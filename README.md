# mongodb-kube  
mongodb-kube は、主にエッジコンピューティング環境において、Kubernetes 上 で MongoDB を立ち上げ稼働させるための、概要説明 と 設定ファイル です。  
MongoDB 構築設定については、本レポジトリ、もしくは、aion-core-manifests の template>bases>mongo 下にある deployment.yml を参照してください。  

## 動作環境  

* OS : Linux OS  
* CPU: ARM/AMD/Intel  
* Kubernetes  
* AION  
  
## MongoDBについて
MongoDBは、クロスプラットフォームのドキュメント指向データベースプログラムです。NoSQLデータベースプログラムとして分類されたMongoDBは、スキーマを持つJSONのようなドキュメントを使用します。AIONは、データエンジニアリング、データ準備、エッジでのIoT/AI環境のコアのプロビジョニングとして、MongoDBのデータスタックを提供します。

エッジ環境はスペックの制限があるため、LatonaおよびAIONでは、機能性とパフォーマンスのバランスに優れているMongoDBが採用されています。
なお、NoSQLには、MongoDBの他にDynamoDB、RedisやFirestoreなどがあります。

- DynamoDB: フルマネージド型のため可用性が高い、開発コストが高い
- Redis: 処理速度が高いが、メモリ消費が高い
- Firestore:　スケーラビリティが高いが、開発コストが高い  

## mongodb-kubeを用いたエッジコンピューティングアーキテクチャ(OMOTE-Bakoアプリケーションの例)  
mongodb-kubeは、下記の黄色い枠の部分のリソースです。  
![mongo_omotebako](docs/omotebako_architecture_20211105.drawio.png)  


## AION における MongoDB 1（メタデータ・ログの維持管理）
AION プラットフォームにおいて、MongoDB は、mongodb-kube として、マイクロサービスとして エッジコンピューティング環境のKubrenetes上で稼働します。   
MongoDB は、IoT や エッジAI の メタデータ・ログの維持管理のために優れており、本レポジトリに加えて、次のようなレポジトリを活用しながら、容易で効率的にデータを維持管理することができます。  

* [fluentd-for-mongodb](https://github.com/latonaio/fluentd-for-mongodb)    

* [fluentd-for-containers-mongodb-kube](https://github.com/latonaio/fluentd-for-containers-mongodb-kube)    

* [insert-metadata-to-mongo](https://github.com/latonaio/insert-metadata-to-mongo)    

## AION における MongoDB 2（データの可視化）  
AION プラットフォームにおいて、次のようなレポジトリを活用しながら、MongoDB内のデータを可視化し、効率的に価値のあるデータをUIに表示することができます。   

* [mongo-express-kube](https://github.com/latonaio/mongo-express-kube)    

* [avis](https://github.com/latonaio/avis)    

## MongoDB のデプロイ・稼働
本レポジトリに mongodb-kube の deployment.yml が含まれています。
また、[aion-core-manifests](https://github.com/latonaio/aion-core-manifests)の template/bases/mongo/deployment.yml に、AIONにおいて MongoDB をデプロイ・稼働させるために必要なyamlファイルが配置されています。

## ymlファイル（deployment.yml）の中身  
ymlファイル（deployment.yml）の中身  

```      
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo
spec:
  selector:
    matchLabels:
      app: mongo
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
        - image: mongo:5.0
          name: mongo
          resources:
            limits:
              memory: 512Mi
              cpu: 100m
            requests:
              memory: 100Mi
              cpu: 10m
          env:
            - name: MONGODB_USER
              value: root
            - name: MONGODB_PASS
              value: root
          ports:
            - containerPort: 27017
              name: mongo
          volumeMounts:
            - name: mongo-persistent-storage
              mountPath: /data/db
      volumes:
        - name: mongo-persistent-storage
          persistentVolumeClaim:
            claimName: mongo-pv-claim
```  
