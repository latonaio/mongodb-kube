## 概要
AION環境で MongoDB を立ち上げ稼働させるための、概要説明です。  
MongoDB 構築設定については、下記、もしくは、aion-core-manifests の template>bases>mongo 下にある deployment.yml を参照してください。

## 動作環境
・ OS : Linux OS  
・ CPU: ARM/AMD/Intel  
・ Kubernetes  
・ AION  

## MongoDBについて
MongoDBは、クロスプラットフォームのドキュメント指向データベースプログラムです。NoSQLデータベースプログラムとして分類されたMongoDBは、スキーマを持つJSONのようなドキュメントを使用します。AIONは、データエンジニアリング、データ準備、エッジでのIoT/AI環境のコアのプロビジョニングとして、MongoDBのデータスタックを提供します。

エッジ環境はスペックの制限があるため、LatonaおよびAIONでは、機能性とパフォーマンスのバランスに優れているMongoDBが採用されています。
なお、NoSQLには、MongoDBの他にDynamoDB、RedisやFirestoreなどがあります。

- DynamoDB: フルマネージド型のため可用性が高い、開発コストが高い
- Redis: 処理速度が高いが、メモリ消費が高い
- Firestore:　スケーラビリティが高いが、開発コストが高い  

## AION における MongoDB のデプロイ・稼働
[aion-core-manifests](https://github.com/latonaio/aion-core-manifests)の template>bases>mongo の deployment.yml に MongoDB をデプロイ・稼働させるために必要なyamlファイルが配置されています。

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
        - image: mongo:4.4
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

## MongoDBについて
MongoDBは、クロスプラットフォームのドキュメント指向データベースプログラムです。NoSQLデータベースプログラムとして分類されたMongoDBは、スキーマを持つJSONのようなドキュメントを使用します。AIONは、データエンジニアリング、データ準備、エッジでのIoT/AI環境のコアのプロビジョニングとして、MongoDBのデータスタックを提供します。

エッジ環境はスペックの制限があるため、LatonaおよびAIONでは、機能性とパフォーマンスのバランスに優れているMongoDBが採用されています。
なお、NoSQLには、MongoDBの他にDynamoDB、RedisやFirestoreなどがあります。

- DynamoDB: フルマネージド型のため可用性が高い、開発コストが高い
- Redis: 処理速度が高いが、メモリ消費が高い
- Firestore:　スケーラビリティが高いが、開発コストが高い  


