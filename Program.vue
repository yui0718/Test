/* eslint-disable no-irregular-whitespace */
<template>
  <div class="app">
    <div class="row" style="height:800px;">
      <div class="col-4 m-3 border-right">
        ラジオ局
        <select v-model="station">
          <option value selected></option>
          <option value="LFR">ニッポン放送</option>
          <option value="TBS">TBSラジオ</option>
          <option value="QRR">文化放送</option>
          <option value="FMT">TOKYO FM</option>
          <option value="BAYFM78">bayfm</option>
          <option value="NACK5">NACK5</option>
          <option value="802">FM 802</option>
          <option value="OBC">ラジオ大阪</option>
          <option value="ABC">ABCラジオ</option>
          <option value="MBS">MBSラジオ</option>
          <option value="RN1">ラジオNIKKEI第1</option>
        </select>
        <div class="row mt-3 mb-3">
          <div class="col-md-8">
            <span class="demonstration"></span>
            <el-date-picker
              v-model="date"
              type="date"
              placeholder="Pick a day"
              value-format="yyyyMMdd"
            ></el-date-picker>
          </div>
          <div class="col-md-4">
            <input type="button" class="btn btn-info" value="表示" @click="onProgram" />
          </div>
        </div>
        <ul v-for="(value,key) of Programs" :key="key">
          <li>
            <el-popover placement="right-end" title="番組詳細" width="400" trigger="hover" content>
              <div v-html="value.desc"></div>
              <div v-html="value.info"></div>
              <router-link
                to="#"
                slot="reference"
                @click.native="StationToRec_Shell(value,station)"
              >{{value.title}} {{value.start.substring(8,12)}}-{{value.end.substring(8,12)}}</router-link>
            </el-popover>
          </li>
        </ul>
      </div>
      <div class="col-4 ml-0" style="height:100%;">
        <select class="form-control mt-3" v-model="pref_id">
          <option value selected>都道府県</option>
          <option value="all">全国</option>
          <option value="JP1">北海道</option>
          <option value="JP2">青森県</option>
          <option value="JP3">岩手県</option>
          <option value="JP4">宮城県</option>
          <option value="JP5">秋田県</option>
          <option value="JP6">山形県</option>
          <option value="JP7">福島県</option>
          <option value="JP8">茨城県</option>
          <option value="JP9">栃木県</option>
          <option value="JP10">群馬県</option>
          <option value="JP11">埼玉県</option>
          <option value="JP12">千葉県</option>
          <option value="JP13">東京都</option>
          <option value="JP14">神奈川県</option>
          <option value="JP15">新潟県</option>
          <option value="JP16">富山県</option>
          <option value="JP17">石川県</option>
          <option value="JP18">福井県</option>
          <option value="JP19">山梨県</option>
          <option value="JP20">長野県</option>
          <option value="JP21">岐阜県</option>
          <option value="JP22">静岡県</option>
          <option value="JP23">愛知県</option>
          <option value="JP24">三重県</option>
          <option value="JP25">滋賀県</option>
          <option value="JP26">京都府</option>
          <option value="JP27">大阪府</option>
          <option value="JP28">兵庫県</option>
          <option value="JP29">奈良県</option>
          <option value="JP30">和歌山県</option>
          <option value="JP31">鳥取県</option>
          <option value="JP32">島根県</option>
          <option value="JP33">岡山県</option>
          <option value="JP34">広島県</option>
          <option value="JP35">山口県</option>
          <option value="JP36">徳島県</option>
          <option value="JP37">香川県</option>
          <option value="JP38">愛媛県</option>
          <option value="JP39">高知県</option>
          <option value="JP40">福岡県</option>
          <option value="JP41">佐賀県</option>
          <option value="JP42">長崎県</option>
          <option value="JP43">熊本県</option>
          <option value="JP44">大分県</option>
          <option value="JP45">宮崎県</option>
          <option value="JP46">鹿児島県</option>
          <option value="JP47">沖縄県</option>
        </select>
        <br />
        <input type="text" v-model="word" class="form-control" @keydown.enter="onProgramSearch" />
        <ul>
          <p v-if="flg" class="mt-3">タイムフリー</p>
          <ul v-for="(value,key) of pastPrograms.data" :key="key">
            <li>
              <el-popover placement="right-end" title="番組詳細" width="400" trigger="hover" content>
                <div v-html="value.description"></div>
                <div v-html="value.info"></div>
                <router-link
                  to="#"
                  slot="reference"
                  @click.native="SearchToRec_Shell(value)"
                >{{value.program_date}} {{value.start_time_s}}-{{value.end_time_s}} {{value.title}} {{value.station_id}}</router-link>
              </el-popover>
            </li>
          </ul>
          <br />
          <p v-if="flg">オンエア予定</p>
          <ul v-for="(value,key) of futurePrograms.data" :key="key">
            <li>
              <el-popover placement="right-end" title="番組詳細" width="400" trigger="hover" content>
                <div v-html="value.description"></div>
                <div v-html="value.info"></div>
                <div
                  slot="reference"
                >{{value.program_date}} {{value.start_time_s}} {{value.title}} {{value.station_id}}</div>
              </el-popover>
            </li>
          </ul>
        </ul>
      </div>
      <div class="col-3 mt-3 border-left">
<p>録音番組リスト</p>
<el-collapse v-model="activeNames" @change="handleChange">
  <el-collapse-item title="月曜日" name="1">
    <div>            <router-link
              to="#"
              @click.native="ProgramToRec_Shell('上沼恵美子のこころ晴天','JP27',1)"
            >上沼恵美子のこころ晴天</router-link></div>
    <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('壇蜜の耳蜜','JP13',1)"
            >壇蜜の耳蜜</router-link></div>
            <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('AKB48 Team 8 今夜は帰らない･･･','JP23',1)"
            >今夜は帰らない…</router-link></div>
                      <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('水樹奈々のＭの世界','JP13',1)"
            >水樹奈々のＭの世界</router-link></div>      
                      <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('かまいたちのヘイ！タクシー！','JP13',1)"
            >かまいたちのヘイ！タクシー！</router-link></div>
          <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('前田亘輝YOU達HAPPY','JP13',1)"
            >前田亘輝YOU達HAPPY</router-link></div>
          <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('SCHOOL OF LOCK！','JP13',1)"
            >SCHOOL OF LOCK！</router-link></div>
          <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('よなよな…月曜日','JP27',1)"
            >よなよな…月曜日</router-link></div>
                      <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('ミルクボーイの煩悩の塊','JP27',1)"
            >ミルクボーイの煩悩の塊</router-link></div>                          
                                    <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('MIDNIGHT GARAGE(24時台)','JP27',1)"
            >MIDNIGHT GARAGE(24時台)</router-link></div>
            <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('MIDNIGHT GARAGE(25時台)','JP27',1)"
            >MIDNIGHT GARAGE(25時台)</router-link></div>
                        <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('MIDNIGHT GARAGE(26時台)','JP27',1)"
            >MIDNIGHT GARAGE(26時台)</router-link></div>
             <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('ファーストサマーウイカのオールナイトニッポン0','JP13',1)"
            >ファーストサマーウイカのオールナイトニッポン0</router-link></div>
  </el-collapse-item>
    <el-collapse-item title="火曜日" name="2">
                      <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('エレマガラジオDX','JP27',2)"
            >エレマガラジオDX</router-link></div>
                      <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('よなよな… 火曜日','JP27',2)"
            >よなよな…火曜日</router-link></div>
          
 
                                <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('アルコ＆ピース D.C.GARAGE','JP13',2)"
            >アルコ＆ピース D.C.GARAGE</router-link></div>
                               <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('爆笑問題カーボーイ','JP13',2)"
            >爆笑問題カーボーイ</router-link></div>
                                    <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('Poppin\'FLAG!!!(24時台)','JP27',2)"
            >Poppin'FLAG!!!(24時台)</router-link></div>
                                                <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('Poppin\'FLAG!!!(25時台)','JP27',2)"
            >Poppin'FLAG!!!(25時台)</router-link></div>
                                                <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('Poppin\'FLAG!!!(26時台)','JP27',2)"
            >Poppin'FLAG!!!(26時台)</router-link></div>
                        <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('星野源のオールナイトニッポン','JP13',2)"
            >星野源のオールナイトニッポン</router-link></div>
                         <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('Creepy Nutsのオールナイトニッポン0','JP13',2)"
            >Creepy Nutsのオールナイトニッポン0</router-link></div>
  </el-collapse-item>
      <el-collapse-item title="水曜日" name="3">
                                       <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('EVENING TAP(18時台)','JP27',3)"
            >EVENING TAP(18時台)</router-link></div>
                                                    <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('EVENING TAP(19時台)','JP27',3)"
            >EVENING TAP(19時台)</router-link></div>
                                            <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('EVENING TAP(20時台)','JP27',3)"
            >EVENING TAP(20時台)</router-link></div>
                                <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('乃木坂46のオールナイトニッポン','JP13',3)"
            >乃木坂46のオールナイトニッポン</router-link></div>
                                <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('佐久間宣行のオールナイトニッポン0','JP13',3)"
            >佐久間宣行のオールナイトニッポン0</router-link></div>
  </el-collapse-item>
      <el-collapse-item title="木曜日" name="4">
                                        <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('EVENING TAP(18時台)','JP27',4)"
            >EVENING TAP(18時台)</router-link></div>
                                                    <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('EVENING TAP(19時台)','JP27',4)"
            >EVENING TAP(19時台)</router-link></div>
                                            <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('EVENING TAP(20時台)','JP27',4)"
            >EVENING TAP(20時台)</router-link></div>
                                                              <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('銀シャリのほくほくマネーラジオ','JP13',4)"
            >銀シャリのほくほくマネーラジオ</router-link></div>
                                                            <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('ハライチのターン！','JP13',4)"
            >ハライチのターン！</router-link></div>
                                                   <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('ナインティナインのオールナイトニッポン','JP13',4)"
            >ナインティナインのオールナイトニッポン</router-link></div>
                                                      
                                                      <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('水溜りボンドのオールナイトニッポン0','JP13',4)"
            >水溜りボンドのオールナイトニッポン0</router-link></div>
  </el-collapse-item>
      <el-collapse-item title="金曜日" name="5">
                                       <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('山田啓二のローカルフィット','JP26',5)"
            >山田啓二のローカルフィット</router-link></div>
                                         <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('ザ・ラジオショー（13時台）','JP13',5)"
            >ザ・ラジオショー（13時台）</router-link></div>
            <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('ザ・ラジオショー（14時台）','JP13',5)"
            >ザ・ラジオショー（14時台）</router-link></div>
            <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('ザ・ラジオショー（15時台）','JP13',5)"
            >ザ・ラジオショー（15時台）</router-link></div>
            <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('ザ・ラジオショー（エンディング）','JP13',5)"
            >ザ・ラジオショー（エンディング）</router-link></div>
                             <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('福島のぶひろの、金曜でいいんじゃない？','JP27',5)"
            >福島のぶひろの、金曜でいいんじゃない？</router-link></div>
                    <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('AWESOME FRIDAYS(18時台)','JP27',5)"
            >AWESOME FRIDAYS(18時台)</router-link></div>
          
                    <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('AWESOME FRIDAYS(19時台)','JP27',5)"
            >AWESOME FRIDAYS(19時台)</router-link></div>
          
                    <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('AWESOME FRIDAYS(20時台)','JP27',5)"
            >AWESOME FRIDAYS(20時台)</router-link></div>
                                                             <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('コウテイの銀蛾・流電音','JP27',5)"
            >コウテイの銀蛾・流電音</router-link></div>
            <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('West Side Story','JP34',5)"
            >あ～ちゃん ちゃあぽんの！”West Side Story”</router-link></div>
                               <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('ヤマヒロのぴかいちラジオ','JP27',5)"
            >ヤマヒロのぴかいちラジオ</router-link></div>
                                            <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('LiSA LOCKS!','JP13',5)"
            >LiSA LOCKS!</router-link></div>
                             <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('めっちゃすきやねん','JP27',5)"
            >めっちゃすきやねん</router-link></div>
            <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('ROCK KIDS 802-FRIDAY＆SATURDAY-(21時台)','JP29',5)"
            >ROCK KIDS 802-FRIDAY＆SATURDAY-(21時台)</router-link></div>
                                           <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('ROCK KIDS 802-FRIDAY＆SATURDAY-(22時台)','JP29',5)"
            >ROCK KIDS 802-FRIDAY＆SATURDAY-(22時台)</router-link></div>
                                           <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('ROCK KIDS 802-FRIDAY＆SATURDAY-(23時台)','JP29',5)"
            >ROCK KIDS 802-FRIDAY＆SATURDAY-(23時台)</router-link></div>
                                                            <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('週刊ヤングフライデー','JP27',5)"
            >週刊ヤングフライデー</router-link></div>
            <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('和牛のモーモーラジオ','JP13',5)"
            >和牛のモーモーラジオ</router-link></div>
                    <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('三四郎のオールナイトニッポン','JP13',5)"
            >三四郎のオールナイトニッポン</router-link></div>
                  <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('バナナマンのバナナムーンGOLD','JP13',5)"
            >バナナマンのバナナムーンGOLD</router-link></div>
                    <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('霜降り明星のオールナイトニッポン0','JP13',5)"
            >霜降り明星のオールナイトニッポン0</router-link></div>
  </el-collapse-item>
      <el-collapse-item title="土曜日" name="6">
                <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('土曜朝6時 木梨の会。','JP13',6)"
            >土曜朝6時 木梨の会。</router-link></div>
            <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('トレンディエンジェルのPePePeラジオ','JP13',6)"
            >トレンディエンジェルのPePePeラジオ</router-link></div>
            <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('あみるのママで','JP27',6)"
            >あみるのママで</router-link></div>
                       <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('さまぁ～ず三村マサカズと小島瑠璃子の「みむこじラジオ！」','JP13',6)"
            >さまぁ～ず三村マサカズと小島瑠璃子の「みむこじラジオ！」</router-link></div>
                                   <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('上坂すみれの♡をつければかわいかろう','JP13',6)"
            >上坂すみれの♡をつければかわいかろう</router-link></div>
            <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('ROCK KIDS 802-FRIDAY＆SATURDAY-(22時台)','JP29',6)"
            >ROCK KIDS 802-FRIDAY＆SATURDAY-(22時台)</router-link></div>
                                           <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('ROCK KIDS 802-FRIDAY＆SATURDAY-(23時台)','JP29',6)"
            >ROCK KIDS 802-FRIDAY＆SATURDAY-(23時台)</router-link></div>
                                           <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('ROCK KIDS 802-FRIDAY＆SATURDAY-(24時台)','JP29',6)"
            >ROCK KIDS 802-FRIDAY＆SATURDAY-(24時台)</router-link></div>
                       <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('エレ片のコント太郎','JP13',6)"
            >エレ片のコント太郎</router-link></div>
                       <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('オードリーのオールナイトニッポン','JP13',6)"
            >オードリーのオールナイトニッポン</router-link></div>
                       <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('霜降り明星のだましうち！','JP27',6)"
            >霜降り明星のだましうち！</router-link></div>
                    <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('802 Palette(26時台)','JP27',6)"
            >802 Palette(26時台)</router-link></div>
           <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('802 Palette(27時台)','JP27',6)"
            >802 Palette(27時台)</router-link></div>
                       <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('802 Palette(28時台)','JP27',6)"
            >802 Palette(28時台)</router-link></div>
  </el-collapse-item>
      <el-collapse-item title="日曜日" name="7">
        <div>
            <router-link to="#" @click.native="ProgramToRec_Shell('安住紳一郎の日曜天国','JP13',0)">安住紳一郎の日曜天国</router-link>
          </div>
          <div>
            <router-link to="#" @click.native="ProgramToRec_Shell('乃木坂46の「の」','JP13',0)">乃木坂46の「の」</router-link>
          </div>
          <div>
            <router-link to="#" @click.native="ProgramToRec_Shell('日向坂46の「ひ」','JP13',0)">日向坂46の「ひ」</router-link>
          </div>
                                <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('Grace Place(19時台)','JP27',0)"
            >Grace Place(19時台)</router-link></div>
                                  <div><router-link
              to="#"
              @click.native="ProgramToRec_Shell('Grace Place(20時台)','JP27',0)"
            >Grace Place(20時台)</router-link></div>
          <div>
            <router-link
              to="#"
              @click.native="ProgramToRec_Shell('有吉弘行のSUNDAY NIGHT DREAMER','JP7',0)"
            >有吉弘行のSUNDAY NIGHT DREAMER</router-link>
          </div>
          <div>
            <router-link
              to="#"
              @click.native="ProgramToRec_Shell('松山千春 ON THE RADIO','JP13',0)"
            >松山千春 ON THE RADIO</router-link>
          </div>
          <div>
            <router-link
              to="#"
              @click.native="ProgramToRec_Shell('水瀬いのり MELODY FLAG','JP13',0)"
            >水瀬いのり MELODY FLAG</router-link>
          </div>
          <div>
            <router-link to="#" @click.native="ProgramToRec_Shell('東海オンエアラジオ','JP23',0)">東海オンエアラジオ</router-link>
          </div>
          <div>
            <router-link
              to="#"
              @click.native="ProgramToRec_Shell('高橋みなみと朝井リョウ ヨブンのこと','JP13',0)"
            >高橋みなみと朝井リョウ ヨブンのこと</router-link>
          </div>
          <div>
            <router-link
              to="#"
              @click.native="ProgramToRec_Shell('鈴木敏夫のジブリ汗まみれ','JP13',0)"
            >鈴木敏夫のジブリ汗まみれ</router-link>
          </div>
                       <div>
            <router-link
              to="#"
              @click.native="ProgramToRec_Shell('MUSIC FREAKS(22時台)','JP29',0)"
            >MUSIC FREAKS(22時台)</router-link> 
          </div>
                    <div>
            <router-link
              to="#"
              @click.native="ProgramToRec_Shell('MUSIC FREAKS(23時台)','JP29',0)"
            >MUSIC FREAKS(23時台)</router-link> 
          </div>
                    <div>
            <router-link
              to="#"
              @click.native="ProgramToRec_Shell('水樹奈々スマイル ギャング','JP13',0)"
            >水樹奈々 スマイルギャング</router-link> 
          </div>
                              <div>
            <router-link
              to="#"
              @click.native="ProgramToRec_Shell('HERE COMES THE MOON','JP13',0)"
            >HERE COMES THE MOON</router-link> 
          </div>
          <div>
            <router-link to="#" @click.native="ProgramToRec_Shell('なまらぶ','JP1',0)">なまらぶ</router-link>
          </div>
  </el-collapse-item>
</el-collapse>
          
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
export default {
  data: function() {
    return {
      result: "",
      flg: false,
      pref_id: "",
      word: "",
      URL: "",
      pastPrograms: [],
      start: "",
      end: "",
      futurePrograms: [],
      Programs: [],
      date: new Date(),
      station: ""
    };
  },
  methods: {
          handleChange(val) {
        console.log(val);
      },
        onProgramSearch: function(event) {
      if (event.keyCode != 13) return;
      this.flg = true;
      if (this.pref_id != "all"){
              this.URL =
        "http://radiko.jp/v3/api/program/search?key=" +
        this.word +
        "&filter=past&start_day=&end_day=&area_id=" +
        this.pref_id +
        "&region_id=&cul_area_id=JP13&page_idx=0&uid=e3948e8de187bab420a2fb4487f35675&row_limit=12&app_id=pc&action_id=0";
      axios.get(this.URL, { headers: this.headers }).then(response => {
        this.pastPrograms = response.data;
      });
      }else{
      this.URL =
        "http://radiko.jp/v3/api/program/search?key=" +
        this.word +
        "&filter=past&start_day=&end_day=&area_id=&region_id=&cul_area_id=all&page_idx=0&uid=e3948e8de187bab420a2fb4487f35675&row_limit=12&app_id=pc&action_id=0";
      axios.get(this.URL, { headers: this.headers }).then(response => {
        this.pastPrograms = response.data;
            });
      }
      if (this.pref_id != "all"){
      this.URL =
        "http://radiko.jp/v3/api/program/search?key=" +
        this.word +
        "&filter=future&start_day=&end_day=&area_id=" +
        this.pref_id +
        "&region_id=&cul_area_id=JP13&page_idx=0&uid=e3948e8de187bab420a2fb4487f35675&row_limit=12&app_id=pc&action_id=0";
      axios.get(this.URL, { headers: this.headers }).then(response => {
        this.futurePrograms = response.data;
      });
      }else{
              this.URL =
        "http://radiko.jp/v3/api/program/search?key=" +
        this.word +
        "&filter=future&start_day=&end_day=&area_id=&region_id=&cul_area_id=all&page_idx=0&uid=e3948e8de187bab420a2fb4487f35675&row_limit=12&app_id=pc&action_id=0";
      axios.get(this.URL, { headers: this.headers }).then(response => {
        this.futurePrograms = response.data;
      });
      }
    },
    SearchToRec_Shell: function(value) {
      const remote = require("electron").remote;
      const app = remote.app;
      this.start = value.start_time
        .replace(/-/g, "")
        .replace(/\s+/g, "")
        .replace(/:/g, "")
        .substring(0, 12);
      this.end = value.end_time
        .replace(/-/g, "")
        .replace(/\s+/g, "")
        .replace(/:/g, "")
        .substring(0, 12);
      const execFileSync = require("child_process").execFile;
      execFileSync(
        '"' + app.getPath("userData") + '/rec_radiko_ts.sh"',
        [
          "-s " + value.station_id + "",
          "-f " + this.start + "",
          "-t " + this.end + "",
          '-o "/Users/YUI/Desktop/' +
            value.title +
            "_" +
            this.start.substring(0, 4) +
            "年" +
            this.start.substring(4, 6) +
            "月" +
            this.start.substring(6, 8) +
            '日.m4a"',
          '-m "xxx_yyy_www_1234@yahoo.co.jp"',
          '-p "yuihiroki"'
        ],
        { shell: true },
        (error, stdout, stderr) => {
          if (error) return console.error("ERROR", error);
          console.error("STDERR", stderr);
          alert("" + value.title + ":録音完了");
        }
      );
    },
    StationToRec_Shell: function(value, station) {
      const remote = require("electron").remote;
      const app = remote.app;
      const execFileSync = require("child_process").execFile;
      execFileSync(
        '"' + app.getPath("userData") + '/rec_radiko_ts.sh"',
        [
          "-s " + station + "",
          "-f " + value.start + "",
          "-t " + value.end + "",
          '-o "' +
            app.getPath("desktop") +
            "/" +
            value.title +
            "_" +
            value.start.substring(0, 4) +
            "年" +
            value.start.substring(4, 6) +
            "月" +
            value.start.substring(6, 8) +
            '日.m4a"',
          '-m "xxx_yyy_www_1234@yahoo.co.jp"',
          '-p "yuihiroki"'
        ],
        { shell: true },
        (error, stdout, stderr) => {
          if (error) return console.error("ERROR", error);
          console.error("STDERR", stderr);
          alert("" + value.title + ":録音完了");
        }
      );
    },
    ProgramToRec_Shell: function(title, area, week) {
      var date;
      var now = new Date();
      for (var i = 0; i < 7; i++) {
        date = new Date(now.getFullYear(), now.getMonth(), now.getDate() - i);
        var wDay = date.getDay();
        if (week == wDay) {
          date = moment(date).format("YYYY-MM-DD");
          console.log(date)
          break;
        }
      }
      this.URL =
        "http://radiko.jp/v3/api/program/search?key=" +
        title +
        "&filter=past&start_day=" +
        date +
        "&end_day=&area_id=" +
        area +
        "&region_id=&cul_area_id=JP13&page_idx=0&uid=e3948e8de187bab420a2fb4487f35675&row_limit=12&app_id=pc&action_id=0";
      axios.get(this.URL, { headers: this.headers }).then(response => {
        this.SearchToRec_Shell(response.data.data[0]);
      });
    },
    onProgram: function() {
      let self = this;
      this.URL =
        "http://radiko.jp/v3/program/station/date/" +
        this.date +
        "/" +
        this.station +
        ".xml";
      axios
        .get(this.URL, {
          timeout: 3000,
          responseType: "document"
        })
        .then(function(response) {
          const xml = response.data;
          const area = xml.getElementsByTagName("radiko")[0];
          var ulElement = area.getElementsByTagName("progs")[0];

          var count = ulElement.childElementCount;
          self.Programs.length = 0;
          for (var i = 0; i < count; i++) {
            self.Programs.push({
              info: unescapeHTML(
                area.getElementsByTagName("info")[i].innerHTML
              ),
              title: area.getElementsByTagName("title")[i].innerHTML,
              start: area
                .getElementsByTagName("prog")[i].getAttribute("ft")
                .substring(0, 12),
              desc: unescapeHTML(
                area.getElementsByTagName("desc")[i].innerHTML
              ),
              end: area
                .getElementsByTagName("prog")[i].getAttribute("to")
                .substring(0, 12)
            });
          }
        });
      function unescapeHTML(escapedHtml) {
        const doc = new DOMParser().parseFromString(escapedHtml, "text/html");
        return doc.documentElement.textContent;
      }
    }
  }
};
</script>

  <style scoped>
.item {
  margin-top: 10px;
  margin-right: 40px;
}
</style>