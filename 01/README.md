# コンテナ概要

「コンテナ技術」（あるいは単に「コンテナ」）と言った際には、様々な定義がありました（あります）。
2020 年現在では、コンテナ技術の標準化が進むと共に、ある程度の共通見解ができてきています。

ここでは、2020 年の Web の文脈において単純に「コンテナ技術」と言った際にどのような意味合いを持つのか、
また、現在のコンテナ技術がどのようにして成り立っているのかについての概要を説明します。

## コンテナ技術とは

コンテナ技術とは、アプリケーションとそれを動かすために必要な依存関係すべてをパッケージ化し、
そのパッケージをなるべく独立して動作させるような技術のことを指しています。

コンテナ技術は、大きく分けて「コンテナイメージ」と「コンテナランタイム」という 2 つの技術で成り立っています。
これら 2 つの仕様は、Open Container Initiative (OCI) によって標準化が行われています。

(refs: [What is a Container?](https://www.docker.com/resources/what-container))
(refs: [Understanding Linux containers](https://www.redhat.com/en/topics/containers))

## コンテナイメージ

コンテナ技術における、アプリケーションと依存関係を全て含むようなパッケージのことを「コンテナイメージ」と呼びます。

コンテナイメージは、アプリケーションの実行に必要な全てのファイルを含んでいるため、
コンテナを動作させる環境さえ用意すれば、様々なアプリケーションをその環境で動かすことができるようになっています。

コンテナイメージのフォーマットは、[opencontainers/image-spec](https://github.com/opencontainers/image-spec) にて定められています。

## コンテナランタイム

コンテナイメージを動作させる環境のことを「コンテナランタイム」と呼びます。

コンテナランタイムの仕様は、[opencontainers/runtime-spec](https://github.com/opencontainers/runtime-spec) にて定められています。

コンテナランタイムには様々な実装が存在し、実装ごとに様々な特徴があります。

- [opencontainers/runc](https://github.com/opencontainers/runc)
    - Linux のプロセスとしてコンテナを実行する
        - プロセスレベルの分離のため起動は高速
        - ホスト OS の脆弱性などの影響を受けやすい
    - 2020 年現在 Docker 標準のコンテナランタイムとして利用されている
- [google/gvisor (runsc)](https://github.com/google/gvisor)
    - ユーザ空間上にカーネルを提供し、その上でコンテナを実行する
    - アプリケーションから呼ばれるシステムコールは監視・制限される
    - 起動は高速だがアプリケーションの動作にオーバーヘッドなどがある
        - gVisor で実装されていないシステムコールもある
- [kata-containers/runtime](https://github.com/kata-containers/runtime)
    - 仮想マシンを起動させ、その上でコンテナを実行する
        - 分離度が高く他のコンテナの影響を受けにくい
        - 分離度が高くホスト OS の脆弱性の影響などを受けにくい
    - 仮想マシン起動時のオーバーヘッドが高い

(refs: [ユビキタスデータセンターOSの文脈におけるコンテナ実行環境の分類](https://hb.matsumoto-r.jp/entry/2019/02/08/135354))

コンテナランタイムの仕様が標準化されたことにより、ニーズに応じて様々なランタイムを選択したり、実装することができるようになってきました。
今回のインターンシップでは、特に Linux 上のプロセスを用いたコンテナランタイムについて説明し、実装することで、コンテナランタイムへの理解を深めていきます。
