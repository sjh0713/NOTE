## Symfony自带Cache
    <?php
    namespace DataBundle\Service;
    
    use Doctrine\Bundle\DoctrineBundle\Registry;
    
    use Symfony\Component\Cache\Adapter\FilesystemAdapter;
    
    class CacheService
    {
    
        private $doctrine;
    
        public function __construct(Registry $doctrine)
        {
            $this->doctrine = $doctrine;
        }
    
    
        /**
         * 调用方法
         * @param $mode set保存 get获取
         * @param $name 缓存key
         * @param string $time 缓存时间
         * @param array $data 缓存数据
         * @return int|mixed|void
         * @throws \Psr\Cache\InvalidArgumentException
         */
        public function useCache($mode,$name,$time = '',$data = [])
        {
    
            switch ($mode){
                case 'set':
                        $this->setCache($name,$time,$data);
                    break;
                case 'get':
                        return $this->getCache($name);
                    break;
            }
        }
    
        /**
         * cache存储数据
         * @param $name
         * @param $time
         * @param $data
         * @throws \Psr\Cache\InvalidArgumentException
         */
        private function setCache($name,$time,$data)
        {
            $cache = new FilesystemAdapter();
            $total = $cache->getItem($name);
            $total->expiresAfter($time);
            $total->set($data);
            $cache->save($total);
        }
    
        /**
         * cache查看数据
         * @param $name
         * @return int|mixed
         * @throws \Psr\Cache\InvalidArgumentException
         */
        private function getCache($name)
        {
            $cache = new FilesystemAdapter();
            $Online = $cache->getItem($name);
            if (!$Online->isHit()) {
                return 1;
            }
            $Onlinedata = $Online->get();
            return $Onlinedata;
        }
    }