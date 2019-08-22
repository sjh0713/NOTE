##Redis   
   - 使用SncRedisBundle [SncRedisBundle](https://github.com/snc/SncRedisBundle)
   
   - 安装Bundle
          
           composer require snc/redis-bundle 2.1.3
           composer require predis/predis ^1.0  
   
   - 在app/AppKernel.php注册Bundle
   
           public function registerBundles()
            {
               $bundles = [
                   ...
                   new Snc\RedisBundle\SncRedisBundle(),
               ];
            }
   
   - 在app/config/config.yml
   
           snc_redis:
               clients:
                   default:
                       type: phpredis
                       alias: default
                       dsn: redis://localhost:6379
                       # dsn: redis://localhost #无密码
                       logging: "%kernel.debug%"
                   cache:
                       type: phpredis
                       alias: cache
                       dsn: redis://localhost:6379
                       options:
                           profile: 2.2
                           connection_timeout: 10
                           read_write_timeout: 30
                   session:
                       type: phpredis
                       alias: session
                       dsn: redis://localhost:6379
               doctrine:
                   metadata_cache:
                       client: cache
                       entity_manager: default          # the name of your entity_manager connection
                       document_manager: default        # the name of your document_manager connection
                   result_cache:
                       client: cache
                       entity_manager: default  # you may specify multiple entity_managers
                   query_cache:
                       client: cache
                       entity_manager: default
                   second_level_cache:
                       client: cache
                       entity_manager: default
               session:
                   client: session
                   prefix: session_
                   ttl: 3600
                   locking: true
                   spin_lock_wait: 150000          
        
   - 服务调用
   
           # 控制器调用服务
            $redis = $this->get('snc_redis.default');
            # 存
                # 永久有效
                $redis->set('key','value');
                # 改为1小时有效
                $redis->expire('key', 3600);
                # 存并设置1小时有效(EX为单位:秒)
                $redis->set('key','value', 'EX', 3600);
            
            # 取
                $value = $redis->get('key');
            
            # hash存
                # 永久有效
                $redis->hset('main_key', 'key','value');
                # 改为1小时有效
                $redis->expire('main_key', 3600);
            
            # hash取
                $value = $redis->hget('main_key', 'key');
            
            # 集合存
                $redis->lpush('key', 'value');
            
            # 集合取(0首个元素, -1最终元素, 范围自由指定)
                $value = $redis->lrange('key', 0, -1);
            
            # 删
                $redis->del('key');
            
            # 键匹配
                $keys = $redis->keys('ke*');
                print_r($keys);
                 
   - Query Builder
   
            return $this->createQueryBuilder('r')
                  ->select('r.name')
                  ->where('r.id IN(:ids)')
                  ->setParameter('ids', $ids, \Doctrine\DBAL\Connection::PARAM_INT_ARRAY)
                  ->getQuery()
                  ->useQueryCache(true)
                  ->useResultCache(true, 86400, 'custom_cache_id')
                  ->getArrayResult();