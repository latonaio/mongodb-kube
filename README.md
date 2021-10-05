## 概要
Kubernetes上でMongoDBを立ち上げるためのマイクロサービスです。
立ち上げに必要なマニフェストファイルが入っています。

## 動作環境
mongodb-kubeは、kubernetes上での動作を前提としています。 kubernetesの環境構築後に起動してください。

## マニフェストファイルの仕様
- ポート: 27017
- コンテナイメージ: mongo:4.4
- volumeのマウント場所
    - persistentVolume:
        - コンテナ: /data/db
        - hostOS:  /mnt/mongo_data

## MongoDBについて
MongoDBは、クロスプラットフォームのドキュメント指向データベースプログラムです。NoSQLデータベースプログラムとして分類されたMongoDBは、スキーマを持つJSONのようなドキュメントを使用します。AIONは、データエンジニアリング、データ準備、エッジでのIoT/AI環境のコアのプロビジョニングとして、MongoDBのデータスタックを提供します。

エッジ環境はスペックの制限があるため、LatonaおよびAIONでは、機能性とパフォーマンスのバランスに優れているMariaDB(MySQL)を採用されています。
なお、NoSQLには、MongoDBの他にDynamoDB、RedisやFirestoreなどがあります。

- DynamoDB: フルマネージド型のため可用性が高い、開発コストが高い
- Redis: 処理速度が高いが、メモリ消費が高い
- Firestore:　スケーラビリティが高いが、開発コストが高い