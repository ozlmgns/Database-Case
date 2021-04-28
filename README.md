# Database-Case
Database Case

Kurulumlar</br>
https://www.couchbase.com/downloads</br>
https://www.docker.com/products/docker-desktop</br>
`python3 -m pip install couchbase`</br>

İlk olarak Docker CLI'da couchbase'i çalıştıralım.</br>
`docker run -d -p 8091-8093:8091-8093 couchbase`</br>
`docker logs db`</br>
Tarayıcıdan, http: // localhost: 8091 kullanarak Web Konsoluna erişin . Konteyner çalışır durumdaysa, Couchbase Sunucu Kurulum Ekranını görmelisiniz.</br>
![image](https://user-images.githubusercontent.com/66086595/116462662-5faf6380-a872-11eb-8689-737b3741e747.png)</br>
Yönergeleri izleyerek cluster'ımızı kuruyoruz. </br>
![image](https://user-images.githubusercontent.com/66086595/116462894-b0bf5780-a872-11eb-96ad-1c82bc16e266.png)</br>
Ardından bir dockerfile dosyası ve onunla aynı konumda bir configure.sh dosyası oluşturuyoruz.</br>
configure.sh dosyamıza bunu ekliyoruz.</br>
`set -m`</br>
`/entrypoint.sh couchbase-server &`</br>
`sleep 15`</br>
`curl -v -X POST http://127.0.0.1:8091/pools/default -d memoryQuota=512 -d indexMemoryQuota=512`</br>
`curl -v http://127.0.0.1:8091/node/controller/setupServices -d services=kv%2cn1ql%2Cindex`</br>
 `curl -v http://127.0.0.1:8091/settings/web -d port=8091 -d username=ozlmgns -d password=password curl -i -</br>u``$COUCHBASE_ADMINISTRATOR_USERNAME:ozlmgns -X POST http://127.0.0.1:8091/settings/indexes -d 'storageMode=memory_optimized'`</br>
`curl -v -u $COUCHBASE_ADMINISTRATOR_USERNAME:ozlmgns -X POST http://127.0.0.1:8091/pools/default/buckets -d name=$COUCHBASE_BUCKET -d` </br>`bucketType=couchbase -d ramQuotaMB=128 -d authType=sasl -d saslPassword=$COUCHBASE_BUCKET_PASSWORD`</br>
`fg 1`</br>
Artık bir yapılandırma betiğimiz olduğuna göre , Dockerfile dosyasını tamamlayabilir ve özel imajımızı oluşturabiliriz. Dockerfile dosyasını açıp, aşağıdakini ekliyoruz.</br>
`FROM couchbase`</br>
`COPY configure.sh /opt/couchbase`</br>
`CMD ["/opt/couchbase/configure.sh"]`</br>
Şimdi ise imajımızı oluşturmak için docker cli'da aşağıdakini çalıştırıyoruz.</br>
`docker build -t couchbase-custom /path/to/directory/with/dockerfile`</br>
`docker run -d -p 8091-8093:8091-8093 couchbase-custom`</br>
Her konteynere bir isim vermemiz ve konteynerlerin üzerinde çalıştığı ağı tanımlamamız gerekiyor. </br>
`docker run -d \`</br>
    ` -p 8091-8093:8091-8093 \`</br>
    ` -e COUCHBASE_ADMINISTRATOR_USERNAME=ozlmgns \`</br>
    `-e COUCHBASE_ADMINISTRATOR_PASSWORD=password \`</br>
    `-e COUCHBASE_BUCKET=default \`</br>
    `-e COUCHBASE_BUCKET_PASSWORD= \`</br>
    `--network="docker_default" \`</br>
    `--name couchbase1 couchbase-custom`</br>
`docker run -d`</br>
    `-p 9091-9093:8091-8093 \`</br>
    `-e COUCHBASE_ADMINISTRATOR_USERNAME=ozlmgns \`</br>
    `-e COUCHBASE_ADMINISTRATOR_PASSWORD=password \`</br>
    ` -e COUCHBASE_BUCKET=default \`</br>
    ` -e COUCHBASE_BUCKET_PASSWORD= \`</br>
    `--network="docker_default" \`</br>
    ` --name couchbase2 couchbase-custom`</br>
    Konteynırları birbirine bağlayalım.</br>
    `docker exec -it couchbase1 bash`</br>
    
    
   
    
    
    
    
    


    
    
   
