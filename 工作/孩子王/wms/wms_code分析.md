##项目访问流程
访问 http://localhost:8080/wms-web/，有过滤器配置不拦截静态资源，比如js ，配置了首页index.html




index.html加载了所用到的全部js文件








所以当访问页面的时候都会经过main-controller.js，会判断当前用户是否登录，以及用户的权限，根据用户权限加载左侧菜单。
以下是加载的三级菜单的信息：




左侧菜单的数据来源


```
/**
     * 查询用户有权访问的系统菜单
     *
     *  @param  reqDto 查询请求
     *  @return  系统菜单
     */
     @Override
     public  List<FirstClassMenu> selectSysMenuList(SysMenuReqDto  reqDto ) {
        List<FirstClassMenu>  menuList  =  sysModuleDao .selectFirstClassMenu( reqDto );
         for  ( int   i  = 0;  menuList  !=  null  &&  i  <  menuList .size(); ++ i ) {
             reqDto .setParentSysNo( menuList .get( i ).getSysNo());
            List<SecondClassMenu>  secondMenuList  =  sysModuleDao .selectSecondClassMenu( reqDto );
             if  ( null  !=  secondMenuList  && ! secondMenuList .isEmpty()) {
                 for  (SecondClassMenu  secondMenu  :  secondMenuList ) {
                     reqDto .setParentSysNo( secondMenu .getSysNo());
                    List<SecondClassMenu>  thirdMenuList  =  sysModuleDao .selectSecondClassMenu( reqDto );
                     secondMenu .setThirdModules( thirdMenuList );
                }
                 menuList .get( i ).setSubModules( secondMenuList );
            }
        }
         return   menuList ;     }   



/**
     * 查询一级菜单
     *
     *  @param  reqDto 查询条件
     *  @return  一级菜单
     */
     @Override
     public  List<FirstClassMenu> selectFirstClassMenu(SysMenuReqDto  reqDto ) {
         try  {
             return   this .getMySqlSessionTemplate().selectList( NAMESPACE  +  ".selectFirstClassMenu" ,  reqDto );
        }  catch  (Exception  e ) {
             e .printStackTrace();
             throw   new  WMS3UnCheckedException(WMS3ExceptionCode. DB_OPERATOR_EXPT .getCode(),  e );
        }
    }
     /**
     * 查询二级菜单
     *
     *  @param  reqDto 查询条件
     *  @return  二级菜单
     */
     @Override
     public  List<SecondClassMenu> selectSecondClassMenu(SysMenuReqDto  reqDto ) {
         try  {
             return   this .getMySqlSessionTemplate().selectList( NAMESPACE  +  ".selectSecondClassMenu" ,  reqDto );
        }  catch  (Exception  e ) {
             e .printStackTrace();
             throw   new  WMS3UnCheckedException(WMS3ExceptionCode. DB_OPERATOR_EXPT .getCode(),  e );
        }     }  


SysModuleMapper.xml


<!--查询一级菜单 (Daniel Kong) -->
     < select   id = "selectFirstClassMenu"   parameterType = "com.haiziwang.kwms.common.domain.dto.sys.SysMenuReqDto"   resultMap = "FirstResultMap" >
        SELECT
          sys_no, module_code, module_name, url, controller, template, parent_sys_no, order_id
        FROM sys_module
        WHERE parent_sys_no IS NULL
            AND yn = 1
            ORDER BY
              parent_sys_no, order_id
     </ select >
     <!--查询二级菜单 (Daniel Kong) -->
     < select   id = "selectSecondClassMenu"   parameterType = "com.haiziwang.kwms.common.domain.dto.sys.SysMenuReqDto"   resultMap = "SecondResultMap" >
        SELECT
          sys_no, module_code, module_name, url, controller, template, parent_sys_no, order_id
        FROM sys_module
        WHERE parent_sys_no = #{parentSysNo}
            AND yn = 1
            ORDER BY
            parent_sys_no, order_id
     </ select >  

```




##具体模块分析（预约单管理）


switchTab()


```
$rootScope.switchTab =  function  (tabId) {
            $rootScope.$broadcast( 'TAB_ENABLE' , tabId);         };   

```
1.index.html页面引入相应的js文件

2.ibd-appointment-controller.js


对应的类在com.haiziwang.kwms.outbound.controller.IbdAppointmentController.java






##查询预约单列表信息




search()方法在js/controllers/ibd/ ibd-appointment-controller.js




DTInstances  在ibd-appointment-controller.js的首行定义过了
应该是angularJs内部的变量。




分析请求：


后台请求：


 











##新增预约




##预约单详情


ibd-appointment-controller.js中定义的方法： showMoadlAppointment()






