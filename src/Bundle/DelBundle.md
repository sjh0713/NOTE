## 删除Bundle  
  1. 删除“src/TestBundle”文件夹
  2. 删除“tests/TestBundle”文件夹
  3. 删除“app/config/config.yml”中：
  
            - { resource: "@TestBundle/Resources/config/services.yml" }
             如果有使用orm
                 orm:
                     default_entity_manager: default
                     entity_managers:
                         default:
                             connection: default
                             mappings:
                                 AppBundle:  ~
                                 TestBundle: ~ 删除该项及其子项
  4. 删除“app/config/routing.yml”中：
             
             Test:
               resource: "@TestBundle/Controller/"
               type: annotation
               prefix: /
  5. 删除“app/AppKernel.php”中：
  
             new TestBundle\TestBundle(),   