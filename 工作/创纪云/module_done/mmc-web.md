##首页广行位的实现


```
1.web.xml
< welcome-file-list >
         < welcome-file > index.do </ welcome-file >
     </ welcome-file-list >
2.HomeAction.java
@Controller
@RequestMapping (value =  "/" , produces =  "text/html;charset=UTF-8" ) public   class  HomeAction  extends  MmcAction {  
@RequestMapping ( "index" )
     public  String home(HttpServletRequest  request , ModelMap  model ) {
        LoginUser  user  = getUserInfo( request );
         if ( null  !=  user .getUserId()){
             user  =  userMgtService .refresh( user );
             if (! user .isMerchant() || ! user .isAudited()){
                 return   "forward:/loginController/toHomePage.do" ;
            }
             if (! user .isAdmin()){
                 return   "forward:/loginController/toAdminPage.do" ;
            }
        }
         model .put( "homePage" ,  cmsService .getHomePage());
         return   "index.ftl" ;
    }
}


@Service (value =  "cmsService" )
public   class  CmsServiceImpl  implements  CmsService {
    
     private   static   final  String  HOME_URI  =  "/publish/998/channel/1.json" ;
    
     @Value ( "${o2o.home.url}" )
     private  String  homeUrl ;
     @Override
     public  HomePage getHomePage() {
        String  result  =  "{}" ;
         try  {
             result  = HttpUtil. doGet ( homeUrl + HOME_URI );
        }  catch  (Exception  e ) {
        }
        JSONObject  jsonObject  = JSONObject. fromObject ( result );
        HomePage  homePage  =  new  HomePage();
         homePage .setSiteId( jsonObject .optInt( "siteId" ));
         homePage .setChannel( jsonObject .optString( "channel" ));
        JSONObject  fJsonObject  =  jsonObject .optJSONObject( "data" );
        Floor[]  floors  =  new  Floor[6];
        Block[]  ads  =  null ;
        Floor  floor  =  null ;
        JSONArray  tempJsonArray  =  null ;
         for ( int   i  = 1;  i  < 7;  i ++){
            JSONObject  tempJsonObj  =  fJsonObject .optJSONObject( "floor" + i );
             if ( null  !=  tempJsonObj ){
                 floor  =  new  Floor();
                 floor .setTitle( tempJsonObj .optJSONObject( "floortitle" + i ).optString( "title" ));
                 floor .setText( tempJsonObj .optJSONObject( "floortitle" + i ).optString( "text" ));
                 floor .setImage( tempJsonObj .optJSONObject( "floorimage" + i ).optString( "image" ));
                 tempJsonArray  =  tempJsonObj .optJSONArray( "floor_in" + i );
                 if ( null  !=  tempJsonArray  &&  tempJsonArray .size() > 0){
                    Block[]  blocks  =  new  Block[ tempJsonArray .size()];
                     for ( int   j  = 0;  j  <  blocks . length ;  j ++){
                         blocks [ j ] = (Block) JSONObject. toBean ( tempJsonArray .getJSONObject( j ), Block. class );
                    }
                     floor .setBlocks( blocks );
                }
                 floors [ i -1] =  floor ;
            }
        }
         homePage .setFloors( floors );
         tempJsonArray  =  fJsonObject .optJSONArray( "guanggaowei" );
         if ( null  !=  tempJsonArray  &&  tempJsonArray .size() > 0){
             ads  =  new  Block[ tempJsonArray .size()];
             for ( int   j  = 0;  j  <  ads . length ;  j ++){
                 ads [ j ] = (Block) JSONObject. toBean ( tempJsonArray .getJSONObject( j ), Block. class );
            }
        }
         homePage .setAds( ads );
         tempJsonArray  =  jsonObject .optJSONArray( "data" );
         tempJsonArray  =  jsonObject .optJSONArray( "errmsg" );
         if ( null  !=  tempJsonArray  &&  tempJsonArray .size() > 0){
            String[]  errmsg  =  new  String[ tempJsonArray .size()];
             for ( int   i  = 0;  i  <  errmsg . length ;  i ++){
                 errmsg [ i ] =  tempJsonArray .getString( i );
            }
             homePage .setErrmsg( errmsg );
        }
         return   homePage ;
    }
}
mmc-pom/vars.dev.properties:
o2o.home.url= http://192.168.2.55:3000  
 

3.index.ftl:
引入样式与js:
< link   rel = "stylesheet"
     href = " ${resRoot} /common/plugins/slideshow/slideshow.css"   /> < script   src = " ${resRoot} /common/plugins/slideshow/slideshow.js" ></ script >  
 

<!-- bootstrap轮播 开始-->
     < div   class = "comiis_wrapad"   id = "slideContainer" >
         < div   id = "frameHlicAe"   class = "frame cl" >
             < div   class = "block" >
                 < div   class = "cl" >
                     < ul   class = "slideshow"   id = "slidesImgs" >
                     < #list homePage.ads as block>
                         < li   style =" display :  none ;" >
                             < a >< img   src = " ${block.image} "   alt = "" ></ a >
                         </ li >
                     </ #list>
                     </ ul >
                 </ div >
                 < div   class = "slidebar"   id = "slideBar" >
                     < ul >
                     < #list homePage.ads as block>
                         < #if block_index == 0>
                         < li   class = "on" > 1 </ li >
                         < #else>
                         < li   class = "" > ${block_index+1 } </ li >
                         </ #if>
                     </ #list>
                     </ ul >
                 </ div >
             </ div >
         </ div >
     </ div >
     < script >
        SlideShow(2000);
     </ script >
     <!-- bootstrap轮播 结束-->


slideshow.js:

function  SlideShow(c) {
     var  a = document.getElementById( "slideContainer" ), f = document.getElementById( "slidesImgs" ).getElementsByTagName( "li" ), h = document.getElementById( "slideBar" ), 
    n = h.getElementsByTagName( "li" ), d = f.length, c = c || 3000, e = lastI = 0, j, m;
     function  b() {
        m = setInterval( function  () {
            e = e + 1 >= d ? e + 1 - d : e + 1;
            g()
        }, c)
    }
     function  k() {
        clearInterval(m)
    }
     function  g() {
        f[lastI].style.display =  "none" ;
        n[lastI].className =  "" ;
        f[e].style.display =  "block" ;
        n[e].className =  "on" ;
        lastI = e
    }
    f[e].style.display =  "block" ;
    a.onmouseover = k;
    a.onmouseout = b;
    h.onmouseover =  function  (i) {
        j = i ? i.target : window.event.srcElement;
         if  (j.nodeName ===  "LI" ) {
            e = parseInt(j.innerHTML, 10) - 1;
            g()
        }
    };
    b()
};




点击互联网支付跳到对应的floor
< ul   class = "nab-ul" >
                 < li >< a   href = "#"   id = "list1"   class = "active" > 首页 </ a ></ li >
                 < #list homePage.floors as floor>
                 < li >< a   href = "javascript:scroller('f ${floor_index+1 } ', 800)"   id = "list ${floor_index+2 } " > ${floor.text } </ a ></ li >
                 </ #list>              </ ul >  


function  intval(v){
    v = parseInt(v);
     return  isNaN(v) ? 0 : v;
}  // ?取元素信息   
function  getPos(e){
     var  l = 0;
     var  t = 0;
     var  w = intval(e.style.width);
     var  h = intval(e.style.height);
     var  wb = e.offsetWidth;
     var  hb = e.offsetHeight;
     while  (e.offsetParent) {
        l += e.offsetLeft + (e.currentStyle ? intval(e.currentStyle.borderLeftWidth) : 0);
        t += e.offsetTop + (e.currentStyle ? intval(e.currentStyle.borderTopWidth) : 0);
        e = e.offsetParent;
    }
    l += e.offsetLeft + (e.currentStyle ? intval(e.currentStyle.borderLeftWidth) : 0);
    t += e.offsetTop + (e.currentStyle ? intval(e.currentStyle.borderTopWidth) : 0);
     return  {
        x: l,
        y: t,
        w: w,
        h: h,
        wb: wb,
        hb: hb
    };
}  // ?取??条信息   
function  getScroll(){
     var  t, l, w, h;
     if  (document.documentElement && document.documentElement.scrollTop) {
        t = document.documentElement.scrollTop;
        l = document.documentElement.scrollLeft;
        w = document.documentElement.scrollWidth;
        h = document.documentElement.scrollHeight;
    }
     else
         if  (document.body) {
            t = document.body.scrollTop;
            l = document.body.scrollLeft;
            w = document.body.scrollWidth;
            h = document.body.scrollHeight;
        }
     return  {
        t: t,
        l: l,
        w: w,
        h: h
    };
}  // ?点(Anchor)?平滑跳?   
function  scroller(el, duration){
     if  ( typeof  el !=  'object' ) {
        el = document.getElementById(el);
    }
     if  (!el)
         return ;
     var  z =  this ;
    z.el = el;
    z.p = getPos(el);
    z.s = getScroll();
    z.clear =  function (){
        window.clearInterval(z.timer);
        z.timer =  null
    };
    z.t = ( new  Date).getTime();
    z.step =  function (){
         var  t = ( new  Date).getTime();
         var  p = (t - z.t) / duration;
         if  (t >= duration + z.t) {
            z.clear();
            window.setTimeout( function (){
                z.scroll(z.p.y, z.p.x)
            }, 13);
        }
         else  {
            st = ((-Math.cos(p * Math.PI) / 2) + 0.5) * (z.p.y - z.s.t) + z.s.t;
            sl = ((-Math.cos(p * Math.PI) / 2) + 0.5) * (z.p.x - z.s.l) + z.s.l;
            z.scroll(st, sl);
        }
    };
    z.scroll =  function (t, l){
        window.scrollTo(l, t)
    };
    z.timer = window.setInterval( function (){
        z.step();
    }, 13);
}  

```
