##foot.ftl






##menu.ftl








##CommonUtil  .java



##IndexController.java
```
/**
 * SUNING APPLIANCE CHAINS.
 * Copyright (c) 2012 - 2012 All Rights Reserved.
 */
package  com.bn.b2b.omsSupplier;
import  java.io.UnsupportedEncodingException;
import  java.lang.reflect.InvocationTargetException;
import  java.util.ArrayList;
import  java.util.List;
import  org.apache.commons.beanutils.BeanUtils;
import  org.apache.commons.lang3.StringUtils;
import  org.springframework.beans.factory.annotation.Autowired;
import  org.springframework.stereotype.Controller;
import  org.springframework.ui.Model;
import  org.springframework.web.bind.annotation.RequestMapping;
import  org.springframework.web.servlet.View;
import  com.bn.b2b.omsSupplier.entity.Menu;
import  com.bn.b2b.omsSupplier.entity.Menu2;
import  com.bn.b2b.omsSupplier.intf.service.SysMenuService;
import  com.bn.framework.uaa.common.model.TreeNode;
import  com.bn.framework.uaa.common.system.UaaSystemInfoGetter;
import  com.bn.framework.uaa.core.manager.SysMenuManager;
import  com.bn.framework.web.gson.GsonView;
/**
 * 〈一句话功能简述〉 <br>
 * Controller 〈功能详细描述〉跳转index页面
 */
@ Controller
@ RequestMapping( "/" )
public   class  IndexController2 {
     @ Autowired
    SysMenuService sysMenuService;
     @ Autowired
    SysMenuManager sysMenuManager;
     @ Autowired
     private  UaaSystemInfoGetter uaaSystemInfoGetter;
     @ RequestMapping(value =  "findAllJsonMenu" )
     public  View findAllJsonMenu(Model model,Long treeRootId,  boolean  cleanCache)  throws  UnsupportedEncodingException {
         if (treeRootId== null ) {
            treeRootId=1L;
        }
        String system = uaaSystemInfoGetter.getSystemName();
        List<TreeNode> treeList = sysMenuService.findAllJsonMenu(system);
        List<Menu> menuList = changeListToTreeData(treeList);
         long  rootId = 0;
        List<Menu2> menu2List =  new  ArrayList<Menu2>();
         if (menuList !=  null ){
             for (Menu menu : menuList){
                 if (menu.getParentId() == 0L){
                    rootId = menu.getId();
                     break ;
                }
            }
            List<Menu> subMenuList =  null ;
            Menu2 menu2 =  null ;
             for (Menu menu : menuList){
                 if (menu.getParentId() != 0L){
                     if (menu.getParentId().equals(rootId)){
                        menu2 =  new  Menu2();
                        subMenuList =  new  ArrayList<Menu>();
                        menu2.setSubMenuList(subMenuList);
                        menu2.setMenu(menu);
                        menu2List.add(menu2);
                    } else  {
                        subMenuList.add(menu);
                    }
                }
            }
        }
        model.addAttribute( "menus" , menu2List);
        
         return   new  GsonView( "menus" ,  null );
    }
     private  List<Menu> changeListToTreeData(List<TreeNode> treeList){
        List<Menu> menuList =  new  ArrayList<Menu>();
         for  (TreeNode item : treeList) {
             if  (StringUtils.isEmpty(item.getUrl())) {
                item.setUrl( null );
            } else   if  (StringUtils.startsWith(item.getUrl(),  "NO_URL" )) {
                item.setUrl( null );
            }  else   if  (StringUtils.startsWith(item.getUrl(),  "/" )) {
                item.setUrl(item.getUrl().substring(1));
            }
            Menu menuItem =  new  Menu();
             try  {
                BeanUtils.copyProperties(menuItem,item);
                menuItem.setTarget( "mainFrame" );
                menuList.add(menuItem);
            }  catch  (IllegalAccessException e) {
                e.printStackTrace();
            }  catch  (InvocationTargetException e) {
                e.printStackTrace();
            }
        }
         return  menuList;
    }
}
```
## PreAuthenticationFilter.java
```
package  com.bn.b2b.omsSupplier.filter;
import  javax.servlet.http.HttpServletRequest;
import  com.bn.framework.uaa.common.access.web.AbstractPreAuthenticationFilter;
import  com.bn.framework.uaa.common.model.Identity;
/**
 * 
 *  @description  权限过滤器
 *  @author  xuneng
 *  @date  2015 - 07 - 013
 */
public   class  PreAuthenticationFilter  extends  AbstractPreAuthenticationFilter  {
     /* (non-Javadoc)
     * @see com.suning.fraamework.uaa.access.web.AbstractPreAuthenticationFilter#getPreAuthenticatedPrincipal(javax.servlet.http.HttpServletRequest)
     */
     @ Override
     protected  Object getPreAuthenticatedPrincipal(HttpServletRequest request) {
         return  request.getRemoteUser();
    }
     /* (non-Javadoc)
     * @see com.suning.framework.uaa.access.web.AbstractPreAuthenticationFilter#successfulAuthentication(java.lang.Object, com.suning.framework.uaa.model.Identity)
     */
     @ Override
     protected  Object successfulAuthentication(Object principal, Identity identity) {
        identity.setId(String.valueOf(principal));
         return  identity;
    }
}
```
##web层相应的配置文件


###web.xml


```
<!--预登陆过滤器  开始  UAA preAuthentication module start -->
    <filter>
        <filter-name>preAuthenticationFilter</filter-name>
        <filter-class>com.bn.b2b.omsSupplier.filter.PreAuthenticationFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>preAuthenticationFilter</filter-name>
        <url-pattern>*.htm</url-pattern>
        <url-pattern>*.do</url-pattern>
    </filter-mapping>
    <!--预登陆过滤器  结束  UAA preAuthentication module end-->
    <!--url 过滤   开始  URL security module start -->
    <filter>
        <filter-name>urlSecurityFilter</filter-name>
        <filter-class>com.bn.framework.uaa.common.access.web.UaaSecurityFilter</filter-class>
        <init-param>
            <param-name>system</param-name>
            <param-value>CRM</param-value>
        </init-param>
        <init-param>
            <param-name>errorPage</param-name>
            <param-value>/authority.html</param-value>
        </init-param>
    </filter>
   <filter-mapping>
       <filter-name>urlSecurityFilter</filter-name>
        <url-pattern>*.htm</url-pattern>
        <url-pattern>*.do</url-pattern>
   </filter-mapping>     <!--url 过滤   结束  URL security module end -->  
``` 



###ehcache.xml

```
<ehcache updateCheck="false" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.sf.net/ehcache.xsd">
    <diskStore path="java.io.tmpdir"/>
    <cacheManagerEventListenerFactory class="" properties=""/>
    
    <!-- multicastGroupPort需要保证与其他系统不重复，请联系权限项目组成员，进行端口注册  -->
    <!-- 若因未注册，配置了重复端口，造成权限缓存数据异常，请自行解决  -->
   <!--  <cacheManagerPeerProviderFactory
            class="net.sf.ehcache.distribution.RMICacheManagerPeerProviderFactory"
            properties="peerDiscovery=automatic,
                        multicastGroupAddress=224.0.0.1,
                        multicastGroupPort=4549, timeToLive=1"/> -->
    <!--配置ehcacheRMI缓存，使其可以在多台服务器 之间缓存同步-->
    <cacheManagerPeerProviderFactory
            class="net.sf.ehcache.distribution.RMICacheManagerPeerProviderFactory"
            properties="peerDiscovery=manual,
                        rmiUrls=//localhost:4567/roleCache|//localhost:4567/childRoleCache
                               |//localhost:4567/parentRoleCache|//localhost:4567/userRoleCache
                               |//localhost:4567/authorizationCache|//localhost:4567/collectionCache
                               |//localhost:4567/appertainedCollectionCache|//localhost:4567/containedCollectionCache
                               |//localhost:4567/authOfCollectionCache|//localhost:4567/collectionOfAuthCache
                               |//localhost:4567/collectionOfRoleCache|//localhost:4567/roleOfcollectionCache"/>
    <cacheManagerPeerListenerFactory
            class="net.sf.ehcache.distribution.RMICacheManagerPeerListenerFactory"
            properties="hostName=localhost, port=4567,socketTimeoutMillis=1000"/>
    <defaultCache maxElementsInMemory="10000"
                  eternal="true"
                  timeToIdleSeconds="0"
                  timeToLiveSeconds="0"
                  overflowToDisk="true"
                  diskPersistent="false"
                  diskExpiryThreadIntervalSeconds="120"
                  memoryStoreEvictionPolicy="LRU"/>
    <cache name="roleCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    <cache name="childRoleCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    <cache name="parentRoleCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    
    <cache name="userRoleCache"
           maxElementsInMemory="5000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    
    <cache name="authorizationCache"
           maxElementsInMemory="50000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    
    <cache name="collectionCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    <cache name="appertainedCollectionCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    <cache name="containedCollectionCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    <cache name="authOfCollectionCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    <cache name="collectionOfAuthCache"
           maxElementsInMemory="50000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    <cache name="collectionOfRoleCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    <cache name="roleOfcollectionCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
</ehcache>
```
## uaa-setting.properties
appVersion= $[project.version]
envName= DEV


##Service层相应的配置


##SysMenuServiceImpl.java
```
/**
 * SUNING APPLIANCE CHAINS.
 * Copyright (c) 2012 - 2012 All Rights Reserved.
 */
package  com.bn.b2b.omsSupplier.service;
import  java.util.List;
import  org.slf4j.Logger;
import  org.slf4j.LoggerFactory;
import  org.springframework.beans.factory.annotation.Autowired;
import  com.bn.b2b.omsSupplier.intf.service.SysMenuService;
import  com.bn.framework.uaa.common.model.TreeNode;
import  com.bn.framework.uaa.core.manager.MenuManager;
import  com.bn.framework.uaa.core.manager.SysMenuManager;
/**
 * 
 * 功能描述：菜单加载接口实现类
 * 
 *  @author  Lin Sen  <a> 11010072@cnsuning.com </a>
 */
//@Service("sysMenuService")
public   class  SysMenuServiceImpl  implements  SysMenuService {
    Logger logger = LoggerFactory.getLogger( this .getClass());
     @ Autowired
    SysMenuManager sysMenuManager;
     @ Autowired
    MenuManager menuManager;
     /*
     * (non-Javadoc)
     * @see com.suning.framework.ucc.menu.SysMenuService#findAllJsonMenu()
     */
     @ Override
     public  List<TreeNode> findAllJsonMenu(String systemId) {
        List<TreeNode> treeNodeList = menuManager.getMenuListBySystemId(systemId.toUpperCase());
         return  treeNodeList;
    }
//    /*
//     * (non-Javadoc)
//     * @see com.suning.framework.ucc.menu.SysMenuService#changeToZTreeData(java.util.List)
//     */
//    @Override
//    public List<TreeNode> changeToZTreeData(List<SimpleJsonTree> sysList) {
//        for (SimpleJsonTree menu : sysList) {
//            if (StringUtils.startsWith(menu.getUrl(), "NO_URL")) {
//                menu.setUrl(null);
//            } else if (StringUtils.startsWith(menu.getUrl(), "/")) {
//                menu.setUrl(menu.getUrl().substring(1));
//            }
////            if (StringUtils.isNotEmpty(menu.getTarget())) {
////                menu.setTarget(menu.getTarget());
////            } else {
//                menu.setTarget("mainFrame");
////            }
//        }
//
//        return sysList;
//    }
//
//    private List<SimpleJsonTree> findAll() {
//        List<SimpleJsonTree> treeDataLst = new ArrayList<SimpleJsonTree>();
//        InputStream input = SysMenuServiceImpl.class.getClassLoader().getResourceAsStream("tree-data.properties");
//        Properties properties = new Properties();
//        try {
//            properties.load(input);
//        } catch (Exception e) {
//            throw new RuntimeException("加载树形菜单数据失败！");
//        }
//
//        for (Object value : properties.values()) {
//            String valStr = (String) value;
//            String[] treeDatas = valStr.split(",");
//            SimpleJsonTree menu = new SimpleJsonTree();
//            menu.setId(treeDatas[0]);
//            menu.setpId(treeDatas[1]);
//            menu.setName(treeDatas[2]);
//            // if(!"NO_URL".equalsIgnoreCase(treeDatas[3])){
//            menu.setUrl(treeDatas[3]);
//            menu.setTarget(treeDatas[4]);
//            // }
//            treeDataLst.add(menu);
//        }
//        return treeDataLst;
//    }
//
//    private List<SimpleJsonTree> sortMenuList(List<SimpleJsonTree> menuList) {
//        Comparator<SimpleJsonTree> comparator = new Comparator<SimpleJsonTree>() {
//            public int compare(SimpleJsonTree s1, SimpleJsonTree s2) {
//                return -Integer.valueOf(s1.getId()).compareTo(Integer.valueOf(s2.getId()));
//            }
//        };
//        Collections.sort(menuList, comparator);
//        return menuList;
//    }
}
```
## common.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:jee="http://www.springframework.org/schema/jee"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/jee http://www.springframework.org/schema/jee/spring-jee.xsd">
<bean class="com.bn.b2b.omsSupplier.service.SysMenuServiceImpl" id="sysMenuService"/>
    <bean id="propertyConfig"
             class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
              <property name="location">
                     <value>classpath:conf/uaa-setting.properties</value>
              </property>
              <property name="systemPropertiesModeName" value="SYSTEM_PROPERTIES_MODE_OVERRIDE" />
       </bean>
       <jee:jndi-lookup id="dataSource2" jndi-name="jdbc/uaa"
                        proxy-interface="javax.sql.DataSource" lookup-on-startup="false" />
       <bean id="uaaDacClient" class="com.bn.framework.dac.client.support.DefaultDacClient">
                 <!-- SQL的Xml配置路径 -->
            <property name="sqlMapConfigLocation">
                     <list>
                            <value>classpath*:conf/sqlMap/sqlMap_uaa_execute.xml</value>
                            <!--如果是 Oracle则使用下面的sql-->
                            <!-- <value>classpath*:conf/sqlMap/sqlMap_uaa_query_oracle.xml</value>
                            <value>classpath*:conf/sqlMap/uaa_sqlMap_page_oraclel.xml</value> -->
                            <value>classpath*:conf/sqlMap/sqlMap_user.xml</value>
                             <!--如果是 Mysql则使用下面的sql-->
                             <value>classpath*:conf/sqlMap/sqlMap_uaa_query_mysql.xml</value>
                             <value>classpath*:conf/sqlMap/uaa_sqlMap_page_mysql.xml</value>
                     </list>
              </property>
              <property name="dataSource" ref="dataSource2"/>
       </bean> </beans>  
```
##uaa-manager.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.bn.framework.uaa" />
    
    <bean id="authConfigurationManager" class="com.bn.framework.uaa.core.manager.ext.AuthConfiguationManagerImpl"/>
    
        <bean id="cacheManager" class="org.springframework.cache.ehcache.EhCacheCacheManager">
        <property name="cacheManager">
            <bean class="org.springframework.cache.ehcache.EhCacheManagerFactoryBean">
                <property name="configLocation" value="classpath:conf/ehcache.xml"/>
                <property name="shared" value="true"/>
                <property name="cacheManagerName" value="uaaCacheManager"/>
            </bean>
        </property>
    </bean>
    <!-- cache的默认实现为ehcache，若项目组有特殊需求，可自行开发或集成其他实现 -->
    <bean id="authorizationManager"
        class="com.bn.framework.uaa.core.manager.AuthorizationManagerImpl">
        <property name="authorizationCache">
            <bean factory-bean="cacheManager" factory-method="getCache">
                <constructor-arg value="authorizationCache" />
            </bean>
        </property>
        <property name="collectionCache">
            <bean factory-bean="cacheManager" factory-method="getCache">
                <constructor-arg value="collectionCache" />
            </bean>
        </property>
        <property name="authOfCollectionCache">
            <bean factory-bean="cacheManager" factory-method="getCache">
                <constructor-arg value="authOfCollectionCache" />
            </bean>
        </property>
        <property name="collectionOfAuthCache">
            <bean factory-bean="cacheManager" factory-method="getCache">
                <constructor-arg value="collectionOfAuthCache" />
            </bean>
        </property>
        <property name="collectionOfRoleCache">
            <bean factory-bean="cacheManager" factory-method="getCache">
                <constructor-arg value="collectionOfRoleCache" />
            </bean>
        </property>
        <property name="roleOfcollectionCache">
            <bean factory-bean="cacheManager" factory-method="getCache">
                <constructor-arg value="roleOfcollectionCache" />
            </bean>
        </property>
        <property name="appertainedCollectionCache">
            <bean factory-bean="cacheManager" factory-method="getCache">
                <constructor-arg value="appertainedCollectionCache" />
            </bean>
        </property>
        <property name="containedCollectionCache">
            <bean factory-bean="cacheManager" factory-method="getCache">
                <constructor-arg value="containedCollectionCache" />
            </bean>
        </property>
    </bean>
    <bean id="roleManager" class="com.bn.framework.uaa.core.manager.RoleManagerImpl">
        <property name="roleCache">
            <bean factory-bean="cacheManager" factory-method="getCache">
                <constructor-arg value="roleCache" />
            </bean>
        </property>
        <property name="childRoleCache">
            <bean factory-bean="cacheManager" factory-method="getCache">
                <constructor-arg value="childRoleCache" />
            </bean>
        </property>
        <property name="parentRoleCache">
            <bean factory-bean="cacheManager" factory-method="getCache">
                <constructor-arg value="parentRoleCache" />
            </bean>
        </property>
        <property name="userRoleCache">
            <bean factory-bean="cacheManager" factory-method="getCache">
                <constructor-arg value="userRoleCache" />
            </bean>
        </property>
    </bean>
</beans>
```
##uaa-security-core.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop" xmlns:security="http://www.springframework.org/schema/security"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
       http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.1.xsd
       http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.0.xsd
      http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security-3.1.xsd">
    <!--【基本服务配置-开始】 -->
    <bean id="systemInfoGetter" class="com.bn.framework.uaa.common.system.UaaSystemInfoGetter">
        <property name="system" value="CSCD" />
    </bean>
    
    <bean id="componentManager" class="com.bn.framework.uaa.common.util.ComponentManager" />
    
    <bean id="authorizationService"
        class="com.bn.framework.uaa.core.service.AuthorizationServiceImpl">
        <property name="authorizationManager" ref="authorizationManager" />
        <property name="roleManager" ref="roleManager" />
        <!-- <property name="userDetailsService" ref="userDetailsService" /> -->
        <property name="selectors">
            <list>
                <ref bean="urlAuthorizationSelector"/>
                <ref bean="standardAuthorizationSelector"/>
            </list>
        </property>
    </bean>
    <bean id="urlAuthorizationSelector"
        class="com.bn.framework.uaa.core.service.selector.UrlAuthorizationSelector">
        <property name="authorizationManager" ref="authorizationManager" />
    </bean>
    <bean id="standardAuthorizationSelector"
        class="com.bn.framework.uaa.core.service.selector.StandardAuthorizationSelector">
        <property name="authorizationManager" ref="authorizationManager" />
        <property name="evaluator">
            <bean class="com.bn.framework.uaa.core.service.expression.SpelEvaluator" />
        </property>
        <property name="conversionService">
            <bean class="org.springframework.context.support.ConversionServiceFactoryBean" />
        </property>
    </bean>
    <!--【基本服务配置-结束】 -->
    <!--【树型结构数据过滤配置-开始】 -->
    <bean id="treeFilterAspect" class="com.bn.framework.uaa.common.access.tree.TreeFilterAspect">
        <property name="filter">
            <bean class="com.bn.framework.uaa.common.access.tree.FlatBasedTreeFilter">
                <property name="authorizationService" ref="authorizationService" />
                <!-- <property name="currentUserInfoGetter" ref="currentUserInfoGetter" /> -->
                <property name="treeNodeInfoExtractor">
                    <bean class="com.bn.framework.uaa.common.access.tree.DefaultTreeNodeInfoExtractor">
                        <property name="idPropertyName" value="id" />
                        <property name="urlPropertyName" value="url" />
                        <property name="systemPropertyName" value="systemId" />
                        <property name="accessPolicy" value="ID_BASED" />
                    </bean>
                </property>
            </bean>
        </property>
    </bean>
    <aop:config proxy-target-class="false">
        <aop:aspect id="treeAspect" ref="treeFilterAspect">
            <aop:pointcut id="target"
                          expression="execution(* com.bn.b2b.omsSupplier.service.SysMenuServiceImpl.findAllJsonMenu(..))" />
            <aop:around method="filter" pointcut-ref="target" />
        </aop:aspect>
    </aop:config>
    <!--【树型结构数据过滤配置-结束】 -->
</beans>
```
##对应的数据库表

 ##接口层的配置



##pom.xml


##Menu.java
```
package  com.bn.b2b.omsSupplier.entity;
import  com.bn.framework.uaa.common.model.TreeNode;
/**
 * Created by kai on 2015/6/17.
 */
public   class  Menu  extends  TreeNode{
     private  String target;
     public  Menu() {
    }
     public  String getTarget() {
         return  target;
    }
     public   void  setTarget(String target) {
         this .target = target;
    }
}
```
##Menu2.java

```
package  com.bn.b2b.omsSupplier.entity;
import  java.util.List;
/**
 * Created by kai on 2015/6/17.
 */
public   class  Menu2 {
     private  Menu menu;
     private  List<Menu> subMenuList;
     public  Menu2() {
    }
     public  Menu getMenu() {
         return  menu;
    }
     public   void  setMenu(Menu menu) {
         this .menu = menu;
    }
     public  List<Menu> getSubMenuList() {
         return  subMenuList;
    }
     public   void  setSubMenuList(List<Menu> subMenuList) {
         this .subMenuList = subMenuList;
    }
}
```
##User.java
```
package  com.bn.b2b.omsSupplier.entity;
import  java.io.Serializable;
/**
 * Created by kai on 2015/6/8.
 */
public   class  User  implements  Serializable {
     private  Integer userId;
     private  String userName;
     private  String nickName;
     public  User() {
    }
     public  Integer getUserId() {
         return  userId;
    }
     public   void  setUserId(Integer userId) {
         this .userId = userId;
    }
     public  String getUserName() {
         return  userName;
    }
     public   void  setUserName(String userName) {
         this .userName = userName;
    }
     public  String getNickName() {
         return  nickName;
    }
     public   void  setNickName(String nickName) {
         this .nickName = nickName;
    }
}
```
## UserQueryForm.java
```
package  com.bn.b2b.omsSupplier.entity;
import  java.io.Serializable;
/**
 * Created by kai on 2015/6/8.
 */
public   class  UserQueryForm  implements  Serializable {
     private  Integer userId;
     private  String userName;
     private  String nickName;
     public  UserQueryForm() {
    }
     public  Integer getUserId() {
         return  userId;
    }
     public   void  setUserId(Integer userId) {
         this .userId = userId;
    }
     public  String getUserName() {
         return  userName;
    }
     public   void  setUserName(String userName) {
         this .userName = userName;
    }
     public  String getNickName() {
         return  nickName;
    }
     public   void  setNickName(String nickName) {
         this .nickName = nickName;
    }
}
```
##SysMenuService.java
```
package  com.bn.b2b.omsSupplier.intf.service;
import  java.util.List;
import  com.bn.framework.uaa.common.model.TreeNode;
/**
 * 
 * 功能描述： Menu相关的类
 * 
 *  @author  作者 陈琦
 * 
 */
public   interface  SysMenuService {
     /**
     * 查询所有菜单信息
     * 
     *  @return  List SimpleJsonTree类型的List为了能使zTree生成树形结构
     */
    List<TreeNode> findAllJsonMenu(String systemId);
     /**
     * 
     * 菜单信息转化成zTree兼容的方式
     * 
     *  @param  sysList
     *  @return  impleJsonTree类型的List为了能使zTree生成树形结构
     */
//    List<TreeNode> changeToZTreeData(List<SimpleJsonTree> sysList);
}
```






 
