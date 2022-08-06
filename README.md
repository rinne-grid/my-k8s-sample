$ kubectl apply -f ./deploy.yaml

# 各リソース

- 同時に作成されるもの
  - Deployment (kubectl get deploy)
  - ReplicaSet (kubectl get rs)
  - Pod (kubectl get pod)
- Service を作成 (kubectl get service)
  - type: LoadBalancer

# 構成

- Deployment
  - template で Pod の構成を指定
    - ラベル
      - app: nginx
    - コンテナイメージ
      - nginx:stable
    - 待ち受けるポート
      - 80
      - nginx イメージに関して、nginx.conf の差し替えを行わないと環境変数が利用できないため、env はコメントアウトしている。
      - 環境変数によるポート変更に対応しているイメージであれば、env による環境変数の設定でいけるはず
  - replicas で Pod の作成数を定義(必ずこの Pod 数を維持するように)
  - selector で制御対象とする Pod を指定
    - 細かい部分は理解できていないが、Deployment の matchLabels で `app: nginx`というラベルを持つ Pod を制御対象とする？
- Service
  - selector で、この Service(LoadBalancer)がルーティングすべき Pod が所属するラベルを指定する
  - Service としては、8000 ポートで待ち受け、80 ポートに割り振る
