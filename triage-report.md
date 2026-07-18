# docker-maven-plugin Issue/PR トリアージレポート

- 対象リポジトリ: **fabric8io/docker-maven-plugin**
- 作成日: **2026-06-28**
- 対象件数: **Issue 95件 / PR 13件**(トリアージ対象は最新100 open issue・全13 open PR。うち #1872 / #1701 / #1679 はその後 close され、#1858 は PR #1919・#1797 は PR #1921・#1428 は PR #1932 のマージでそれぞれ解決したため本レポートから除外)

---

## 1. エグゼクティブサマリー

### disposition 別 件数集計(Issue 95件)

| disposition | 件数 | 区分 |
|---|---:|---|
| keep-needs-impl(要実装・残す) | 34 | 実装が必要 |
| answer-then-close(返信のみで完結) | 18 | 返信のみ |
| needs-maintainer-decision(方針判断) | 13 | 実装が必要(判断後) |
| good-first-issue(軽微な実装) | 10 | 実装が必要(軽微) |
| needs-info(追加情報待ち) | 9 | 返信のみ(回答待ち) |
| close-already-fixed(修正済み) | 4 | 今すぐclose |
| close-stale-obsolete(陳腐化) | 6 | 今すぐclose |
| close-duplicate(重複) | 1 | 今すぐclose |
| **合計** | **95** | |

### おおまかな内訳

- **今すぐ close 候補: 11件**(close-already-fixed 4 + close-stale-obsolete 6 + close-duplicate 1)
- **返信のみで完結: 27件**(answer-then-close 18 + needs-info 9 ※情報依頼の返信で一旦処理可能)
- **実装が必要: 57件**(keep-needs-impl 34 + needs-maintainer-decision 13 + good-first-issue 10)

### まず着手すべき Quick Win

- **修正済み/重複 11件を一括 close**(close-already-fixed・close-stale-obsolete・close-duplicate)。返信文案が用意済みのものはコピペで対応可能。最も簡単に open 数を減らせる。
- **#1710 は報告者自身が #1701 の重複としてクローズ済みと明言** → 確認して即 close。
- **answer-then-close のうち `closeable: now` の質問系 issue(#1841 #1831 #1790 #1777 #1775 #1760 #1758 #1678)** は回答文案ありで即返信→close 可能。
- **needs-info 9件に定型の情報依頼を投げる**(文案あり)。長期無反応のもの(#1716 #1644 #1796 #1726 等)は一定期間後の自動 close 対象として整理。
- **good-first-issue 10件をラベル付け**してコントリビュータ募集に回す(特にドキュメント系 #1895 #1683 #1655 #1715 は着手容易)。
- **PR の close 候補 7件(close-stale 4 + close-superseded 3)を整理**して open PR を 13→6 に圧縮。

---

## 2. PR トリアージ (13件)

| PR# | タイトル(短縮) | 規模 | 状態 | 推奨アクション | 難易度 | 根拠 |
|---|---|---|---|---|---|---|
| [1906](https://github.com/fabric8io/docker-maven-plugin/pull/1906) | docker.build.parallel でスレッドプール並列ビルド | medium | draft-wip | ping-author | hard | Draft放置4ヶ月。機能は有効だがスレッド安全性検証が必要、整形差分が混在 |
| [1855](https://github.com/fabric8io/docker-maven-plugin/pull/1855) | 非標準 build directory を許可 | xs | conflicts | ping-author | medium | 有用な小修正だが changelog コンフリクト+E2E失敗未解決で約16ヶ月放置 |
| [1829](https://github.com/fabric8io/docker-maven-plugin/pull/1829) | commons-io 2.7→2.14.0(it/dockerfile) | xs | mergeable-clean | merge-candidate | trivial | テスト用依存バンプ、MERGEABLE/CLEAN。ただし2.14.0は現在では古い |
| [1794](https://github.com/fabric8io/docker-maven-plugin/pull/1794) | dockerd Builder-Version ヘッダ対応 | small | draft-wip | close-stale | hard | 著者自身がbuildkitはgRPC実装が必要で不十分と結論。CI失敗、実質放棄 |
| [1772](https://github.com/fabric8io/docker-maven-plugin/pull/1772) | runCmds でユーザー変更を許可 | large | conflicts | ping-author | hard | 2年放置・DIRTY・27ファイル差分汚染、Sonar Quality Gate失敗 |
| [1736](https://github.com/fabric8io/docker-maven-plugin/pull/1736) | Runtime healthcheck 定義サポート | large | superseded | close-superseded | hard | 中核機能は別実装で既にmaster取込済み(RunService/ContainerCreateConfig) |
| [1697](https://github.com/fabric8io/docker-maven-plugin/pull/1697) | pullRetries / pushRetries フィールド | medium | conflicts | needs-rebase | medium | 機能は未実装で有効だが CONFLICTING で約3年放置、リベース必要 |
| [1664](https://github.com/fabric8io/docker-maven-plugin/pull/1664) | snakeyaml 1.32→2.0 | xs | superseded | close-superseded | trivial | 現行pomは既に2.3で目的超過達成済み。CONFLICTINGで35ヶ月放置 |
| [1547](https://github.com/fabric8io/docker-maven-plugin/pull/1547) | MemorySwappiness サポート追加 | medium | conflicts | ping-author | medium | 機能は有効だが CONFLICTING で約4年放置、リベース必須 |
| [1429](https://github.com/fabric8io/docker-maven-plugin/pull/1429) | #1428 timestamp parse 例外修正 | small | conflicts | close-stale | medium | CONFLICTING・約4年放置。テストが旧JMockit/JUnit4で現行非互換、著者離職。対象 #1428 は巻き取りの別PR #1932(マージ済み)で解決済み |
| [1421](https://github.com/fabric8io/docker-maven-plugin/pull/1421) | followlogsafterstart | medium | stale | close-stale | hard | 2021年で更新停止・著者が放棄表明。対象コードが大幅改修されリベース不能 |
| [1191](https://github.com/fabric8io/docker-maven-plugin/pull/1191) | docker build --pull オプション対応 | small | conflicts | close-stale | medium | 2020年以降放置・CONFLICTING・CHANGES_REQUESTED。差分は本体取込済みで陳腐化 |
| [1116](https://github.com/fabric8io/docker-maven-plugin/pull/1116) | BuildService リファクタ(Maven/Docker分離) | xl | superseded | close-superseded | hard | 著者(rhuss)WIP。+4443/-4096・165ファイル、service層は独自進化済みで別実装解決 |

### close 推奨 PR(7件)

- **close-stale(4件):** #1794, #1429, #1421, #1191 — いずれも長期放置・コンフリクト・著者放棄/離職で再適用困難。
- **close-superseded(3件):** #1736, #1664, #1116 — 目的が別実装・別バージョンで既に達成済み。

**返信案:**

- **#1794:** 「@rroesch1 こんにちは。このPRは約2年間更新が止まっており、ご自身のコメントの通り単純に 'version' を設定するだけではbuildkitのgRPCセッションを実装しない限りビルドが失敗し得る、という根本的な課題が残っています。現状コードベースにもこの変更は取り込まれていません。一旦このPRはcloseし、buildkit対応はdocker CLI(buildx)利用前提の別Issue/別実装として整理し直すのが良いと考えています。問題なければクローズさせてください。引き続きの議論や再挑戦は歓迎します。」
- **#1429:** 「本PRはありがとうございます。残念ながら master とコンフリクトしており、テストが旧 JMockit/JUnit4 ベースのため現行の JUnit5+Mockito 構成では適用できません。本体の修正方針(timestamp parse 失敗時の fallback)自体は今も有効ですが、メンテナ要望(失敗したログ行全体を warning 出力して regexp を後から調整できるようにする、現在時刻 fallback はクロックドリフトに注意)が未対応で、著者も対応不可とのことです。約4年更新が無く、対象の #1428 は巻き取りの別PR #1932 で修正・マージ済み(#1428 も close 済み)のため、本PRは superseded として close とさせてください。」
- **#1421:** 「@omyl このPRは2021年以降更新が止まっており、現在コンフリクト状態です。対象の StartMojo / wait パッケージはその後大幅にリファクタされ(並行起動の StartedContainer/ExecutorCompletionService、ExitCodeChecker など)、本PRはそのまま再適用できない状態です。当時「テスト未整備で時間切れ」とのことでしたので、いったんクローズさせてください。#1420 の follow-logs 後の終了検知が今も必要であれば、最新 master ベースで新規PRを歓迎します。」
- **#1191:** 「この PR は #626 の派生で2019年作成、2020年9月以降更新が止まっており現在 master とコンフリクト(CONFLICTING)しています。差分に含まれる BuildOptions のクリーンアップ(final 化・diamond operator)は既に本体へ取り込み済みで、ベースイメージの pull 制御も現在は imagePullPolicy / 既存 autoPull で対応されています。当時メンテナ間でもクローズが検討されていた経緯があり、機能が本当に必要であれば最新 master に対して新規 PR として作り直すことを提案します。いったんクローズしてよいでしょうか。 @rohanKanojia」
- **#1736:** 「@poikilotherm ご提案ありがとうございました。本PRの中核である `<run>` への `<healthCheck>` 対応は、その後別の実装でmasterに取り込まれており(RunImageConfiguration/RunService/ContainerCreateConfig 参照)、現状ではコンテナ起動時のヘルスチェック定義が利用可能です。本PRはドラフトのまま長期間更新がなく大きくコンフリクトしているため、superseded として一旦クローズさせてください。Compose 側のヘルスチェック対応など未取り込みの部分にご関心があれば、現行masterベースで新しいPRを頂けると嬉しいです。」
- **#1664:** 「このPRはsnakeyamlを2.0へ上げる内容ですが、現在のmainブランチでは既に2.3が採用されており、本PRの目的は達成済みです。加えてpom.xmlでコンフリクトが発生し約3年更新がありません。close-supersededとしてクローズすることを提案します(@dependabot close)。」
- **#1116:** 「この巨大リファクタPRは2019年4月以降更新が無く、master側のサービス層はその後JibやBuildX対応など大きく作り直されており、本PRが導入予定だったbuild/パッケージ構成も現行コードには存在しません。コンフリクトも全面的で、リベースより作り直しの方が現実的です。役目を終えたものとしてクローズを提案します。意図したリファクタの方向性で今なお有効な部分があれば、現行コードベースに合わせた小さなPRへ分割して再提出いただけると助かります。@rhuss」

### merge 候補 PR(1件 + 著者対応待ち4件)

- **merge-candidate(即マージ可・1件):** #1829 — テスト専用モジュールの commons-io バンプ。`@dependabot recreate` で最新版に更新してからマージ推奨。
  - 返信案: 「テスト専用モジュールの軽微な依存バンプでコンフリクトもなくマージ可能です。ただし2024年作成で2.14.0は現在では古いため、`@dependabot recreate`で最新版に更新してからマージするのが望ましいです。不要であればcloseでも構いません。」
- **ping-author / needs-rebase(著者対応待ち・5件):** #1906, #1855, #1772, #1547, #1697 — 機能自体は有効だがコンフリクト解消・テスト追加・Draft解除などを著者に依頼。一定期間無反応なら close 移行。

---

## 3. Issue トリアージ

### 3.1 今すぐClose候補 (11件)

close-already-fixed / close-stale-obsolete / close-duplicate をまとめて即 close 対象とする。

| Issue# | タイトル | 種別 | 経過月 | 根拠 |
|---|---|---|---:|---|
| [1889](https://github.com/fabric8io/docker-maven-plugin/issues/1889) | docker network run の NPE(docker-ce 29 / rhel9) | close-already-fixed | 7 | ContainerDetails.getIPAddress に Docker 29+(API 1.44+)対応コミット済み。#1887と重複 |
| [1866](https://github.com/fabric8io/docker-maven-plugin/issues/1866) | parallel multi-arch docker buildx | close-already-fixed | 0 | buildAllPlatforms を追加しマージ済み(2026-06-22) |
| [1781](https://github.com/fabric8io/docker-maven-plugin/issues/1781) | it/spring-boot-with-jib 改善 | close-already-fixed | 4 | 既に Spring Boot 2.7.9・GetMapping 化済みで受け入れ条件達成 |
| [1767](https://github.com/fabric8io/docker-maven-plugin/issues/1767) | Charsets.UTF_8 を StandardCharsets に置換 | close-already-fixed | 28 | LogRequestor は既に StandardCharsets.UTF_8 使用。Guava参照無し |
| [1860](https://github.com/fabric8io/docker-maven-plugin/issues/1860) | アラビア語シェルで UnsatisfiedLinkError | close-stale-obsolete | 15 | upstream jnr/jffi のロケール依存バグ。jffi 1.3.15 更新済み、本体対象外 |
| [1732](https://github.com/fabric8io/docker-maven-plugin/issues/1732) | buildx が unknown/unknown イメージ生成 | close-stale-obsolete | 30 | 報告者がプロジェクト終了で情報提供不能・close同意済み |
| [1675](https://github.com/fabric8io/docker-maven-plugin/issues/1675) | 空フォルダ COPY 後の "Layer does not exist" | close-stale-obsolete | 19 | docker/buildkit 側の既知挙動。RUN true 回避策で合意済み、本体対象外 |
| [1674](https://github.com/fabric8io/docker-maven-plugin/issues/1674) | 0.37.0 unauthorized manifest | close-stale-obsolete | 38 | 非常に古い版・3年無反応。レジストリ側権限問題の可能性大 |
| [1662](https://github.com/fabric8io/docker-maven-plugin/issues/1662) | history(本文空) | close-stale-obsolete | 39 | 本文・タイトル空で対応不能、問い合わせにも無反応 |
| [1712](https://github.com/fabric8io/docker-maven-plugin/issues/1712) | jansi AnsiConsole Access Violation | close-stale-obsolete | 32 | Windows+Eclipse/m2e+JDK20 環境依存、2年以上無反応 |
| [1710](https://github.com/fabric8io/docker-maven-plugin/issues/1710) | OrbStack(MacOS)で multi-platform ビルド失敗 | close-duplicate | 32 | 報告者自身が #1701 の重複としてクローズ済みと明言 |

### 3.2 返信のみで完結 / answer-then-close (18件)

| Issue# | タイトル | 根拠 | 返信案 |
|---|---|---|---|
| [1874](https://github.com/fabric8io/docker-maven-plugin/issues/1874) | 2 profile 別ポートで port already allocated | profile の id 重複によるマージで設定衝突。プラグインのバグではない | 2つのprofileの`<id>`が両方とも`docker-integration-test-db-api-7`で重複しています。Mavenは同一idのprofileを1つにマージするため、両方有効化すると設定が混ざりポート衝突が起きます。1つ目を`docker-integration-test-db-api-6`等にリネームしてください。プラグイン側の不具合ではないためクローズします。 |
| [1865](https://github.com/fabric8io/docker-maven-plugin/issues/1865) | build-helper のプロパティが使えない | parse-version が期待フェーズで未実行。順序問題で回避策あり | 原因はbuild-helper-maven-pluginのparse-versionが実行されず`parsedVersion.*`が未解決のままになっているためです。parse-versionの実行フェーズを明示し、docker:build より前に実行されるようにすると解決します。プラグイン側の不具合ではないためクローズします。 |
| [1841](https://github.com/fabric8io/docker-maven-plugin/issues/1841) | buildx で private registry に connection refused | 報告者が driverOpts/network=host で自己解決済み | 報告者ご自身のコメントの通り、buildxのbuilder作成時にホストネットワークを使う設定(`<buildx><driverOpts><network>host</network></driverOpts></buildx>`)で解決できます。解決済みのためcloseします。ドキュメント追記が必要なら別途お知らせください。 |
| [1831](https://github.com/fabric8io/docker-maven-plugin/issues/1831) | 自動ビルドを止めるプロパティは？ | skip/プロファイルで制御可能 | 自動ビルドはgoal実行に紐づきます。`<skip>${docker.skip}</skip>`(または `docker.skip.build`)を設定し、`-Ddocker.skip.build=false` を渡したときだけビルドするか、execution を Maven プロファイル内に置く方法が確実です。 |
| [1790](https://github.com/fabric8io/docker-maven-plugin/issues/1790) | Nexus へ push できない | insecure registry の設定問題。回答済み | これは insecure registry の設定問題です。buildx ビルドでは daemon.json の insecure-registries は使われず、buildkitd.toml で http=true / insecure=true を設定し `<buildx><configFile>` で指定してください。非 buildx の場合は daemon.json を見直してください。 |
| [1777](https://github.com/fabric8io/docker-maven-plugin/issues/1777) | RUN <<EOF サポート | v0.43.4/0.44.0 で動作実績あり。BuildKit 構文の問題 | RUN <<EOF(heredoc)は v0.43.4/0.44.0 で動作実績があります。heredoc は BuildKit が必要なので DOCKER_BUILDKIT=1 もしくは buildx を有効にしてお試しください。再現する場合は最小再現付きで再オープンしてください。 |
| [1775](https://github.com/fabric8io/docker-maven-plugin/issues/1775) | multi-module で packaging=docker が使えない | push は deploy フェーズ紐付け仕様。設定で解決可 | `<packaging>docker</packaging>` では push は deploy フェーズに紐づきます。docker.skip.push / docker.skip で制御し、親 POM では maven-deploy-plugin を skip、push したいモジュールで docker:push を execution として bind してください。 |
| [1760](https://github.com/fabric8io/docker-maven-plugin/issues/1760) | docker:build の verbose 出力方法 | image 定義が無いのが原因。回答済み | 元のconfigに `<images>/<image>` 定義が無く、ビルド対象を検出していなかったのが原因です。`<images><image><name>...</name><build>...</build></image></images>` 形式で設定し、`-Ddocker.verbose=build` を指定してください。 |
| [1758](https://github.com/fabric8io/docker-maven-plugin/issues/1758) | image tag の設定方法 | name にプロパティ展開で対応可 | イメージ名にタグをプロパティとして含める方法(例: `<name>${docker.registry}/.../${name}:${docker.image.tag}</name>`)で `-Ddocker.image.tag=...` を渡せます。`<tags>` はビルド後の追加タグ用です。 |
| [1678](https://github.com/fabric8io/docker-maven-plugin/issues/1678) | multi-arch build で unknown flag: --driver | cli-plugins パス問題。複数名で解決確認済み | docker --config で非標準ディレクトリを指定すると buildx プラグイン解決に失敗するケースです。/usr/local/lib/docker/cli-plugins へのシンボリックリンク作成や docker のアップデートで解消します。環境固有の問題で解決済みのためクローズします。 |
| [1724](https://github.com/fabric8io/docker-maven-plugin/issues/1724) | dockerHost のドキュメント改善要望 | Windows用 npipe 値を案内すれば完結 | Windowsでは `<dockerHost>npipe:////./pipe/docker_engine</dockerHost>` が標準です。スキーマ `npipe://` の付与が必要というご指摘どおりです。ドキュメント追記PRを歓迎します。一旦クローズします。 |
| [1696](https://github.com/fabric8io/docker-maven-plugin/issues/1696) | /bin/bash -c に文字列を渡せない | shell の複数並べが誤り。exec 形式で対応 | `<cmd>`/`<entrypoint>` は単一の `<shell>` 文字列か `<exec><arg>...` のいずれか一方のみ指定できます。/bin/bash -c に文字列を渡す場合は exec 形式(`<arg>/bin/bash</arg><arg>-c</arg><arg>...</arg>`)を使ってください。 |
| [1695](https://github.com/fabric8io/docker-maven-plugin/issues/1695) | marketplace.gcr.io から 401 Unauthorized | buildx が gcloud 認証を引き継がない既知挙動 | これは buildx(docker-container ドライバ)がホストの credential helper を参照しない buildx 側の挙動です。~/.docker/config.json にトークンを静的に書く、`<authConfig>` で明示指定する、docker login で事前取得する、のいずれかをお試しください。 |
| [1692](https://github.com/fabric8io/docker-maven-plugin/issues/1692) | Untagged images が push される | manifest list 構造上の OCI 仕様。バグではない | マルチアーキビルドでは manifest list が各プラットフォーム用 manifest を参照する構造で、ECR 上では untagged 表示になります。これは buildx/OCI の標準仕様です。不要世代の削除には ECR ライフサイクルポリシーをご利用ください。 |
| [1677](https://github.com/fabric8io/docker-maven-plugin/issues/1677) | dockerfile からビルドできない | contextDir/outputDir 同一で tar 自己包含 | "A tar file cannot include itself" は contextDir 内に docker-build.tar が生成され tar が自分自身を含むのが原因です。outputDir をコンテキスト外にするか .maven-dockerignore で除外してください。0.42.1 は古いため最新版での確認もおすすめします。 |
| [1667](https://github.com/fabric8io/docker-maven-plugin/issues/1667) | container startup エラー | コンテナ名衝突。回答済み・無反応 | 同名コンテナが既に存在するのが原因です。既存コンテナを削除(docker rm)するか、コンテナ命名パターン(https://dmp.fabric8.io/#container-name)を設定してください。build+start と external compose の混在も衝突要因です。 |
| [1657](https://github.com/fabric8io/docker-maven-plugin/issues/1657) | no-cache の設定方法 | build 配下 `<noCache>` で対応可 | no-cache は buildOptions ではなく build 配下の `<noCache>true</noCache>` で指定できます(CLI なら `-Ddocker.noCache=true`)。これで解決するはずですのでクローズします。 |
| [1643](https://github.com/fabric8io/docker-maven-plugin/issues/1643) | Rancher/Homebrew docker の buildx 対応 | CLI 差異起因。dockerStateDir 指定で回避可 | `--config` 渡し時の挙動が Docker 公式CLIと Rancher/Homebrew CLI で異なる場合、各CLI側の差異が原因です。回避策として `<buildx><dockerStateDir>~/.docker</dockerStateDir></buildx>` の指定で動作報告があります。当プラグイン対象外として一旦クローズします。 |

### 3.3 追加情報待ち / needs-info (9件)

| Issue# | タイトル | 依頼文案 |
|---|---|---|
| [1899](https://github.com/fabric8io/docker-maven-plugin/issues/1899) | Jib モードで settings.xml 認証を読まない | "credentials were not sent because the connection was over HTTP" は Jib が平文HTTPに認証を送らない仕様です。(1)レジストリがHTTP/HTTPSのどちらか、(2)settings.xml の server id が push 先ホスト(ポート含む)と完全一致か、(3)insecureRegistries 相当の設定有無、をご教示ください。`-X` ログもいただけると助かります。 |
| [1856](https://github.com/fabric8io/docker-maven-plugin/issues/1856) | dependencySet が reactor artifact を解決しない | license-maven-plugin との相互作用の可能性が高いです。調査のため最小再現プロジェクト(pom一式)、各プラグインのバージョン、`mvn -X` の該当ログを共有いただけますか。 |
| [1835](https://github.com/fabric8io/docker-maven-plugin/issues/1835) | buildx が forced push を試みてビルド失敗 | 再現のため、使用中の `<image>` の build/push 設定全体と、`docker:build`/`docker:push` の `-X` 付きログを共有いただけますか。最新版でも再現するかもご確認ください。情報がなければ一定期間後に close します。 |
| [1796](https://github.com/fabric8io/docker-maven-plugin/issues/1796) | JsonObject 期待箇所に JsonArray | inspect API のレスポンスが配列で返った際の例外のようです。(1)どのベースイメージ/レジストリか、(2)最新版 0.46+ でも再現するか、(3)可能なら最小再現プロジェクト、を教えてください。情報がない場合は一旦クローズします。 |
| [1729](https://github.com/fabric8io/docker-maven-plugin/issues/1729) | dangling images を削除できない | Maven 3.9.8 以降で解消した報告があります。まず最新版に更新してお試しください。残る場合は plugin/docker バージョン、`cleanup` 設定、`-Ddocker.verbose=build` 付きの完全なログと再現手順を共有ください。 |
| [1726](https://github.com/fabric8io/docker-maven-plugin/issues/1726) | push で NullPointerException | AuthConfigList.toJson の NPE はメンバー側で再現できていません。レジストリの構築方法(local registry 設定)、最新版(0.45系)でも再発するか、最小再現プロジェクトを共有いただけますか。情報がない場合は一定期間後にクローズします。 |
| [1716](https://github.com/fabric8io/docker-maven-plugin/issues/1716) | ECR 認証に失敗(0.20.1) | バージョン 0.20.1 は非常に古いものです。最新版(0.46.0以降)で同じ問題が再現するかご確認いただけますか。再現する場合はバージョンと実行ログを添えてお知らせください。反応がなければクローズします。 |
| [1682](https://github.com/fabric8io/docker-maven-plugin/issues/1682) | buildx push で 400 Bad Request | レジストリが manifest を拒否した際に多い現象です。(1)レジストリの種類とバージョン、(2)`-X` 付き完全ログ、(3)単一プラットフォームでは成功するか、(4)docker buildx 手動実行で同じエラーが出るか、を教えてください。ご返信ない場合は一定期間後にクローズします。 |
| [1644](https://github.com/fabric8io/docker-maven-plugin/issues/1644) | Auth: illegal base64 data | 調査のため、(1)完全なスタックトレース(`-X` 付き)、(2)`<authConfig>` / settings.xml の設定方法(値はマスク可)、(3)パスワードの特殊文字種類、を共有いただけますか。最新版でも再現するかもご確認ください。情報がなければ一旦クローズします。 |

### 3.4 good first issue / 軽微な実装 (10件)

| Issue# | タイトル | 難易度 | 概要 |
|---|---|---|---|
| [1895](https://github.com/fabric8io/docker-maven-plugin/issues/1895) | MacOS Homebrew CLI Plugin の場所問題を文書化 | trivial | Homebrew で buildx cli-plugin が見つからない既知問題(#1678関連)の回避策(シンボリックリンク手順)をドキュメント追記 |
| [1834](https://github.com/fabric8io/docker-maven-plugin/issues/1834) | Guava 依存の削除 | easy | EnvUtil 等の Guava 利用を JDK 標準へ置換。コントリビュータ着手意思あり |
| [1827](https://github.com/fabric8io/docker-maven-plugin/issues/1827) | buildx cacheTo をスキップするプロパティ追加 | easy | BuildXConfiguration に skipCacheTo 相当フィールド追加。小規模 |
| [1807](https://github.com/fabric8io/docker-maven-plugin/issues/1807) | Retry の対象エラー拡大 | easy | isRetryableErrorCode の条件を HTTP500 のみから I/O・DNS 等へ緩和 |
| [1750](https://github.com/fabric8io/docker-maven-plugin/issues/1750) | docker.platform が `<run>` で効かない | easy | PLATFORM キーが property handler 経由のみ。小修正 or ドキュメント修正 |
| [1722](https://github.com/fabric8io/docker-maven-plugin/issues/1722) | docker.nocache システムプロパティの削除 | trivial | ConfigHelper の非推奨 docker.nocache 削除の小タスク |
| [1715](https://github.com/fabric8io/docker-maven-plugin/issues/1715) | defaultExclusion 設定のドキュメント追加 | trivial | asciidoc に defaultExclusion の記載追記 |
| [1683](https://github.com/fabric8io/docker-maven-plugin/issues/1683) | Multi-Architecture Build ドキュメントの文途切れ修正 | trivial | buildx ドキュメントの文が途中で途切れている明確な不備の修正 |
| [1655](https://github.com/fabric8io/docker-maven-plugin/issues/1655) | default Dockerfile 名の大小文字をドキュメント化 | trivial | デフォルト Dockerfile 探索挙動・大小文字の扱いを明記。コード変更ほぼ不要 |
| [1628](https://github.com/fabric8io/docker-maven-plugin/issues/1628) | autoRemove で No such container 例外 | easy | StopMojo で二重削除時の 404 を握りつぶす小改修。コラボレータ合意済み |

### 3.5 要実装(残す) / keep-needs-impl (34件)

#### easy (7件)

| Issue# | タイトル | 難易度 | 概要 |
|---|---|---|---|
| [1916](https://github.com/fabric8io/docker-maven-plugin/issues/1916) | `<build>` 欠如時に push が失敗 | easy | RegistryService.pushImage が buildConfig==null で push をスキップする実バグ |
| [1914](https://github.com/fabric8io/docker-maven-plugin/issues/1914) | buildx 設定時に `<labels>` が無視される | easy | BuildXService に --label 出力が無い。報告者がPR提供意思あり |
| [1901](https://github.com/fabric8io/docker-maven-plugin/issues/1901) | buildArg maven property が buildX で動かない | easy | docker.buildArg.* の値が buildx 経路で渡らない。最小再現pomあり |
| [1684](https://github.com/fabric8io/docker-maven-plugin/issues/1684) | 0.42.0 の platform プロパティが既定挙動を変更 | easy | start 時に --platform が常にネイティブ設定されエミュレーション動作が壊れる回帰 |
| [1676](https://github.com/fabric8io/docker-maven-plugin/issues/1676) | podman で中間イメージが削除されない | easy | podman compat API は 64桁完全 sha が必須。報告者が REST レベルで原因特定済み |
| [1669](https://github.com/fabric8io/docker-maven-plugin/issues/1669) | logDate=NONE 時の timestamp parse スキップ | easy | logDate=NONE でも createTimestamp を呼び DateTimeParseException で別スレッド落ち |
| [1653](https://github.com/fabric8io/docker-maven-plugin/issues/1653) | buildx が local config.json を上書き | easy | BuildXService が既存 config.json を無条件上書き・削除。報告者が修正案提示済み |

#### medium (25件)

| Issue# | タイトル | 難易度 | 概要 |
|---|---|---|---|
| [1909](https://github.com/fabric8io/docker-maven-plugin/issues/1909) | tar 内ディレクトリ包含でレイヤ無効化 | medium | 親ディレクトリ ./ のタイムスタンプでキャッシュ無効化。再現プロジェクトあり |
| [1907](https://github.com/fabric8io/docker-maven-plugin/issues/1907) | symlink で push 破損(0.47.0+ 回帰) | medium | zip slip 対策起因と思われるシンボリックリンク扱いの回帰。再現手順あり |
| [1905](https://github.com/fabric8io/docker-maven-plugin/issues/1905) | 0.48.1 で pushRegistry が全イメージに適用されない | medium | マルチイメージ時に2番目以降へ未適用で ghcr.io にフォールバックする回帰 |
| [1896](https://github.com/fabric8io/docker-maven-plugin/issues/1896) | build プロパティ変更でコンテナ名インデックスがリセット | medium | %i が再ビルド後に既存コンテナを参照せず1から振り直し命名衝突。最小再現pomあり |
| [1894](https://github.com/fabric8io/docker-maven-plugin/issues/1894) | 0.47.0 で artifact-with-dependencies 回帰 | medium | provided スコープ除外が効かない。assembly bump(#1851)が原因、3.7.1のまま未修正 |
| [1869](https://github.com/fabric8io/docker-maven-plugin/issues/1869) | docker:commit の実装 | medium | docker commit 相当の goal。ImageCommit API で実現可とメンバー合意済み、実装者未確定 |
| [1867](https://github.com/fabric8io/docker-maven-plugin/issues/1867) | dmp:stop でコンテナ削除されず名前を採番しない | medium | stop→remove→start で旧コンテナ残存→409、containerNamePattern で次番号未採番 |
| [1857](https://github.com/fabric8io/docker-maven-plugin/issues/1857) | COPY --from が basic auth を渡さない | medium | プライベートレジストリの認証を渡せない。2025-11 に追従報告あり、buildkit認証連携が必要 |
| [1842](https://github.com/fabric8io/docker-maven-plugin/issues/1842) | Buildkit での再現可能ビルドサポート | medium | BuildXService が --output 未使用で rewrite-timestamp 指定不可。コード箇所特定済み |
| [1822](https://github.com/fabric8io/docker-maven-plugin/issues/1822) | 複数の buildx.cacheFrom 定義 | medium | cacheFrom/cacheTo が単一 String。List 化と CLI 組立変更が必要 |
| [1821](https://github.com/fabric8io/docker-maven-plugin/issues/1821) | container ログが maven ログと混在 | medium | 行単位で混在。並行出力のライン同期/バッファリング改善が必要 |
| [1817](https://github.com/fabric8io/docker-maven-plugin/issues/1817) | AWS SDK 2.x extended-authentication 対応 | medium | 1.x は EOL。JKube に実装済みPR(jkube#3806)があり移植で対応可能 |
| [1806](https://github.com/fabric8io/docker-maven-plugin/issues/1806) | 異なるスコープで同一 ARG を使えない | medium | outer scope の ARG を FROM 行参照時に解決を誤り image 名が不正化 |
| [1792](https://github.com/fabric8io/docker-maven-plugin/issues/1792) | External Compose と Wait が動かない | medium | external compose 時に run/wait 設定が無視される回帰(0.40→0.45)。複数ユーザ報告 |
| [1789](https://github.com/fabric8io/docker-maven-plugin/issues/1789) | コンテナ起動直後に portPropertyFile を書く | medium | 起動直後に書き出す改善。報告者がPR提出意思あり |
| [1784](https://github.com/fabric8io/docker-maven-plugin/issues/1784) | 同一イメージへ複数 name/tag 付与 | medium | エイリアス付与要望。#1444/#526/#1276 と関連 |
| [1770](https://github.com/fabric8io/docker-maven-plugin/issues/1770) | JIB モードで multi-assembly イメージが壊れる | medium | 再現可能なバグ。報告者が修正ブランチ用意、メンバーアサイン済み |
| [1733](https://github.com/fabric8io/docker-maven-plugin/issues/1733) | runtime healthcheck 追加(compose 対応含む) | medium | 報告者が PR 予定と明記 |
| [1711](https://github.com/fabric8io/docker-maven-plugin/issues/1711) | chmod オプションでビルド失敗 | medium | --chmod 等 BuildKit 機能の組み込みサポートが未実装 |
| [1706](https://github.com/fabric8io/docker-maven-plugin/issues/1706) | 複数 wait URL と Basic 認証 | medium | start goal の複数 wait URL と wait 時 Basic 認証が未実装 |
| [1705](https://github.com/fabric8io/docker-maven-plugin/issues/1705) | filter が buildx と併用で効かない | medium | buildx 利用時に filter の変数補間が効かない実バグ |
| [1656](https://github.com/fabric8io/docker-maven-plugin/issues/1656) | ECR push で Docker 認証を EC2 role より優先 | medium | EC2 instance role 優先で skipExtendedAuth でも無効化不可。認証優先順位改善(方針も絡む) |
| [1640](https://github.com/fabric8io/docker-maven-plugin/issues/1640) | buildx が pull 時に registry を無視 | medium | 生成 config に push レジストリ認証しか含まれず pull 用認証が無視される |
| [1639](https://github.com/fabric8io/docker-maven-plugin/issues/1639) | 別タグのイメージでデフォルトコンテナ名が衝突 | medium | コンテナ名生成は simple name、衝突検出は full name のため別タグで衝突 |
| [1680](https://github.com/fabric8io/docker-maven-plugin/issues/1680) | Maven 3.9.2 でプラグイン検証問題 | medium | 複数 Maven 版混在・EOL な Plexus Contextualizable 依存の技術的負債 |

#### hard (2件)

| Issue# | タイトル | 難易度 | 概要 |
|---|---|---|---|
| [1668](https://github.com/fabric8io/docker-maven-plugin/issues/1668) | jib build の Multi-architecture 対応 | hard | 正当な機能要望。メンバーが着手意思確認も返答なし |
| [1633](https://github.com/fabric8io/docker-maven-plugin/issues/1633) | 0.40.3 で Maven build 挙動が変化 | hard | 依存解決変更で package と docker:build の分離実行不可になった回帰。複数ユーザが再現報告 |

### 3.6 方針判断が必要 / needs-maintainer-decision (13件)

| Issue# | タイトル | 論点 |
|---|---|---|
| [1911](https://github.com/fabric8io/docker-maven-plugin/issues/1911) | docker-machine は非推奨 | upstream アーカイブ済みだが、サポート除去/非推奨化は破壊的変更。除去方針の判断が必要 |
| [1886](https://github.com/fabric8io/docker-maven-plugin/issues/1886) | 0.47.0 のデフォルト圧縮(none)が buildkit を破損 | 根本原因が upstream(maven-assembly-plugin)の tar 変化寄り。本体で対応するか判断が必要 |
| [1853](https://github.com/fabric8io/docker-maven-plugin/issues/1853) | docker-credential-gcloud が見つからない(Windows) | 認証ヘルパーの .cmd 拡張子解決。クロスプラットフォーム対応の方針判断が必要 |
| [1844](https://github.com/fabric8io/docker-maven-plugin/issues/1844) | build.outputTimestamp で image creation time 設定 | #1842 と関連。docker API での実現可否・既定挙動の方針判断 |
| [1843](https://github.com/fabric8io/docker-maven-plugin/issues/1843) | Spring Boot 風の効率的コンテナイメージ | レイヤ分割用 assembly descriptor を標準提供するかの判断 |
| [1832](https://github.com/fabric8io/docker-maven-plugin/issues/1832) | podman build が buildx platforms で失敗 | podman は buildx 互換が不完全。podman 対応は方針判断と大きな実装が必要 |
| [1811](https://github.com/fabric8io/docker-maven-plugin/issues/1811) | 0.45 以降 build 失敗(no active sessions 等) | buildkit 強制時に auto-pull が base image を事前 pull しない回帰。実装判断が必要 |
| [1802](https://github.com/fabric8io/docker-maven-plugin/issues/1802) | BuildXConfiguration.isBuildX の改善 | 明示的な buildkit 有効化フラグの是非。ドキュメント不正確の指摘も含む |
| [1785](https://github.com/fabric8io/docker-maven-plugin/issues/1785) | multi-arch ビルドが TLS 環境で失敗 | TLS を無効化できないユーザからの再要望(2025-10)。対応方針の判断が必要 |
| [1725](https://github.com/fabric8io/docker-maven-plugin/issues/1725) | イメージ名を出力する機能 | docker:build -DprintOnly 等の新ゴール/オプション追加の方針判断 |
| [1681](https://github.com/fabric8io/docker-maven-plugin/issues/1681) | カスタム公開プロパティ | poststart スクリプトの値を Maven プロパティへ公開する設計判断 |
| [1648](https://github.com/fabric8io/docker-maven-plugin/issues/1648) | packaging=docker で javadoc が動かない | docker packaging の language を java とすべきか。components.xml 変更の副作用判断 |
| [1630](https://github.com/fabric8io/docker-maven-plugin/issues/1630) | linting 機能要望 | Dockerfile lint(Hadolint等)統合のスコープ拡大判断 |

---

## 4. 推奨される進め方

1. **修正済み・重複・陳腐化の 11件を一括 close(3.1)。** 返信文案が用意済みのものはコピペで即対応。PR 側の close 候補 7件(#1794 #1429 #1421 #1191 #1736 #1664 #1116)も同時に整理し、open PR を 13→6 に圧縮する。
2. **answer-then-close の 18件を返信→close(3.2)。** 特に `closeable: now` の質問系は回答文案ありで即時処理可能。返信即 close で 18件を一掃。
3. **needs-info の 9件に定型の情報依頼を投げる(3.3)。** 長期無反応のもの(#1716 #1644 #1726 #1796 等)はラベル付け+期限を切り、一定期間後に自動 close へ。これで合計 38件(11+18+9)を短期間でクローズまたは待機状態に整理できる。
4. **PR の現役候補を処理(2章)。** #1829 を `@dependabot recreate` 後にマージし、#1906 #1855 #1772 #1547 #1697 は著者へ ping(期限付き)。
5. **good first issue 10件をラベル付けして募集(3.4)、要実装 34件を難易度・回帰/機能で優先度付け(3.5)。** 回帰バグ(#1905 #1907 #1894 #1792 #1684 等)を最優先、good-first-issue のドキュメント系は外部コントリビュータへ。needs-maintainer-decision 13件(3.6)はメンテナ会議の議題としてまとめる。
