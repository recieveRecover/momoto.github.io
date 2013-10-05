# markdown: listに入れ子するcode

http://meta.stackoverflow.com/questions/3792/how-to-nest-code-within-a-list-using-markdown

# urxvt

仮想ゲストのCentOSへ、ホストのArchLinuxのUrxvtからSSH接続すると`Ctrl+l`などの操作で度々エラーが出る。
ホストの.Xdefaultsに次の行を加えたらエラーは出なくなった。

URxvt*termName: rxvt

# ブックマークレット
- Evernote
javascript:(function(){EN_CLIP_HOST='http://www.evernote.com';try{var%20x=document.createElement('SCRIPT');x.type='text/javascript';x.src=EN_CLIP_HOST+'/public/bookmarkClipper.js?'+(new%20Date().getTime()/100000);document.getElementsByTagName('head')[0].appendChild(x);}catch(e){location.href=EN_CLIP_HOST+'/clip.action?url='+encodeURIComponent(location.href)+'&title='+encodeURIComponent(document.title);}})();
- Google Reader
javascript:var%20b=document.body;var%20GR________bookmarklet_domain='https://www.google.co.jp';if(b&&!document.xmlVersion){void(z=document.createElement('script'));void(z.src='https://www.google.co.jp/reader/ui/subscribe-bookmarklet.js');void(b.appendChild(z));}else{location='http://www.google.com/reader/view/feed/'+encodeURIComponent(location.href)}

# Google Search Engine
link: site: allintitle: allintext: allinurl: allinanchor: inanchor:

# Google
<a href="https://plus.google.com/101153525943529592041?rel=author">{{ site.author }}</a>
<a href="https://plus.google.com/101153525943529592041" rel="publisher">{{ site.author }}</a>

# LICENSE
MIT GPT APACHE

# Symbol
{} カーリーブラケット

# PERSONAL COMPUTER
- [MSI N460GTX Cyclone](http://www.msi-computer.co.jp/VGA/N460GTX_Cyclone_OC/)
- [Abee AS-1000B-SR Supremer1000W](http://www.abee.co.jp/Product/PSU/Supremer/SR/index.html)

# Windows 8
dvdshrink32016_jp_setup
Dropbox 1.6.17
VirtualBox-4.2.6-82870-Win
VMware-player-5.0.1-894247
eclipse-SDK-4.2.2-win32-x86_64
Evernote_4.6.2.7927
googledrivesync
KeePassX-0.4.3-win32
SkyDriveSetup
teraterm-4.77
ubuntuone-4.1.91-windows-installer

# gumi#13
redis,rabbitMQ,kombu,nagios

# hikalab
aws, munin, nagios, jenkins, jmeter, maatkit, mysql rds, mk-query-digest, percona toolkit, jet profiler

# 新統合Web環境メモ
KSD "Kona Site Defender"

# Readablecode Memo
表示非営利継承 オライリーjapan タイトル著者出版社ISBN コードは理解しやすくなければいけない 表面上の改善 thesaurus ng: [ tmp,retval,foo ]
loopiterater 状態untrusted** -> trusted**
誤解されない名前 max_,min_,first_,last_,begin_,end_,is,has
一貫性、列、段落 / TODO:,FIXME:,HACK:,XXX: / ループとロジックの単純化, コードの再構成, 選抜テーマ / ハンガリアン記法, ヨーダ記法 / 三項演算子 ? a : b

# blogmemo
文字セット、文字参照

lshal | grep system.hardware.product
sudo dmidecode -s system-product-name

# Marketing Word
- インプレッション
Webサイトに掲載される広告の効果を図る指標の一つで、広告の露出（掲載）回数。
サイトに訪問者が訪れ、広告が１回表示されることを１インプレッションという。
インプレッション１回につき課金する方式をインプレッション保証型広告と呼び、
単価は１０００インプレッションあたりの価格（CPM）で表される。

- DPA [Demand-Side Platform]
オンライン広告において、広告主側の広告効果の最大化を支援するツール。
予算管理、入稿管理、掲載面の管理、ターゲットのユーザ属性に基づいた広告枠の選定、
あるいは過去の成果を反映することで行われる配信条件の最適化といった機能を提供する。
DSPに対し、広告側の販売者側を支援するツールはSSP（Suplly-Side Platform）と呼ばれる。

- CPA [Cost Per Action]
成果報酬型広告、アフィリエイト広告における広告単価の指標で、成果一件あたりの支払額。
広告をクリックするだけでなく、広告主のサイト上で何らかの行動を起こさなければ成果報酬が発生しないため
クリック保証型広告より高価で、成果が発生する件数の割合は低い場合が多い。

- CPC [Cost Per Click] - クリック保証型広告
ネット広告の掲載料金の単位の一つで、クリック１回あたりの料金。
このような課金を行う広告をクリック保証型広告（Pay Per Click Advertising）という。

- CPM [Cost Per Mille]
Webサイトの広告掲載料金の単位の一つで、掲載１０００回あたりの料金。

- ビッグワード
検索エンジンで、多く検索されるキーワード。検索数が少ないものはスモールワードと呼ぶ。
検索される数が多い一方で競合も激しく上位表示が困難。
上位表示させることができれば多くのアクセスを獲得できるが、
利用者のニーズが絞り込まれていないため、コンバージョンに結びつきにくい。
検索連動が他校酷ではビッグワードはクリック単価が高騰しており、費用対効果に見合わないこともある。

- ロングテール
インターネットを用いた物品販売の手法、または概念の一つ。
販売機会の少ない商品でもアイテム数を幅広く取り揃えることで総体としての売上を大きくする。
販売数（population）を縦軸に、商品（product）を横軸にして販売成績の良いものを左から並べて
あまり売れない商品が右側に長く伸びるグラフが「恐竜の尻尾」のように見えることからロングテールと呼ばれる。
売上の８割が全商品のうちの２割で生み出されているとして、パレートの法則と共に用いられることが多い。

- クリエイティブ
広告業界において、広告として掲載するために製作された広告素材などを指す。
インターネット広告においてはバナー広告に使用するための画像そのものを指すことが多い。

- 純広
インターネット広告において、特定の媒体に掲載される広告を意味する。
アドネットワークと通じて複数の媒体へ掲載するネットワーク広告に対する語としてよく用いられる。
また記事広に対して純広と言った場合は、広告主がクリエイティブを用意した広告を指す場合が多い。
記事広は、媒体者側の記事の体裁で編集を加えるタイプの広告である。

- アドネットワーク
インターネット広告のうち、広告媒体のWebサイトを多数集めて「広告配信ネットワーク」を形成し、
その多数のWebサイト上で広告を配信する手法、またはそのネットワークのことをいう。
アドネットワークを提供している主な事業者はアドバタイジングドットコム、サイバーコミュニケーションズ、
デジタルアドバタイジングコンソーシアム、アイメディアドライブなど。

- アドエクスチェンジ
オンライン広告のうち、特定の広告枠におけるインプレッションを入札方式によって売買する方式をいう。
特定の広告枠を対象とするが、いわゆる純広とは異なり、中間業者を介して複数の広告主が入札する。

- リアルタイム入札
オンライン広告の入札の仕組みで、広告のインプレッションが発生する度に広告枠の競争入札を行い
配信する広告を決定する方式のことを言う。
入札希望者はターゲットとなるユーザ属性、広告の掲載基準、掲載面や入札価格などをあらかじめ
設定しておき、実際にインプレッションが発生した場合はもっとも高く入札した広告主の広告を配信する。
広告枠の売り手にとっては複数の広告主に対して広告枠を競売にかけることができ、
また広告主側には、ターゲットに狙いを絞って入札・表示ができるメリットがある。

- ROAS [Return On Advertising Spend]
広告の費用対効果を測るための指標の一つで、売上÷コスト×１００（％）で求める広告掲載料１円あたりの売上額。
この数値が高いほど、費用対効果が高い広告といえる。
オンライン上で販売活動が完結し、かつ価格帯の異なる多種多様な商品を扱う場合に適している。

- CPA [Cost Per Acquisition]
広告の費用対効果を測るための指標の一つで、コスト÷コンバージョン数で求めるコンバージョンの獲得単価。
この数値が低いほど、費用対効果の高い広告といえる。
オンライン上だけでは販売活動が完結せず、扱っている商品の価格幅が狭い場合に適している。

- ROI [Return On Investment]
（コンバージョン数×平均利益単価－コスト）÷コスト×１００（％）で求める投資効果を測るための指標。

