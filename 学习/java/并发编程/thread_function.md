##使用多线程
```
static   int   CRE_SIZE =1000;  
//创建多线程进行券号的生成
         int   totalPage  =0;
         if ( num % CRE_SIZE ==0){
             totalPage = num / CRE_SIZE ;
        }
         else
        {
             totalPage  = ( num / CRE_SIZE )+1;
        }
         log .info( "totalPage=" + totalPage );
        
        BlockingQueue<Runnable>  queue  =  new  LinkedBlockingQueue<Runnable>( totalPage );
        ExecutorService  service  =  new  ThreadPoolExecutor(10, 10, 0L,TimeUnit. MILLISECONDS ,  queue );
        
         for  ( int   i  = 1;  i  <=  totalPage ;  i ++) {
                
             final   int   startS  = ( i -1)* CRE_SIZE ;
             int   end  =  i * CRE_SIZE ;
             //判定是否最后一页
             if ( i == totalPage )
            {
                 end  =  num ;
            }
             final   int   endS  = end ;
        
            Runnable  run  =  new  Runnable() {
                 @Override
                 public   void  run() {
                    System. out .println( "-----------------startS=" + startS + ",endS=" + endS );
                    List<String>  list  =  new  ArrayList<String>();
                     for ( int   i = startS ; i < endS ; i ++)
                    {
                         //生成券号
                        String  date  = DateUtils. format ( new  Date(), Constants. PATTREN_1 );
                         //生成序列号
                        String  no  =  date  + genSeq( i );
                        System. out .println( "[" + i + "]---------------------" + no );
                         list .add( no );
                    }
                    setRedis( ruleId ,  list );
                }
                
                 private   void  setRedis(String  ruleId , List<String>  list ) {
                     //存入redis中去
                     redisClient .execute( list , new  ShardedJedisPipelineAction<Object>(){
                         public  List<Object> doAction(ShardedJedisPipeline  pipeline ,Object  inParam ) {
                             List<String>  nlist = (ArrayList<String>)  inParam ;
                            
                              for (String  s : nlist )
                             {
                                 pipeline .rpush( ruleId ,  s );
                             }    
                              return   pipeline .syncAndReturnAll();
                        }
                    });
                }
            };
             service .execute( run );
        }
         service .shutdown();  

```