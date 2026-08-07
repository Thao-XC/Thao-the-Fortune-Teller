html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Thao's Fortune Telling Shop &mdash; AI Reading</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Ma+Shan+Zheng&family=Cormorant+Garamond:wght@500;600;700&family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root{
    --ink-bg:#12141c; --paper:#ece4d2; --paper-dim:#d9cdae;
    --seal:#a3382a; --seal-dark:#7c2a20; --gold:#b8935a; --ink-text:#211d18;
  }
  *{box-sizing:border-box;}
  body{
    margin:0; min-height:100vh;
    background:
      radial-gradient(ellipse at 20% -10%, #262a3c 0%, transparent 55%),
      radial-gradient(ellipse at 100% 110%, #241c22 0%, transparent 50%),
      var(--ink-bg);
    color:var(--paper); font-family:'Inter', sans-serif;
    display:flex; justify-content:center; padding:40px 16px 60px;
  }
  .wrap{width:100%; max-width:600px;}
  .brush{font-family:'Ma Shan Zheng', cursive; font-size:52px; color:var(--gold); text-align:center; line-height:1; margin-bottom:2px;}
  h1{font-family:'Cormorant Garamond', serif; font-weight:600; font-size:26px; letter-spacing:0.03em; text-align:center; margin:0 0 4px;}
  .subtitle{text-align:center; font-size:12px; letter-spacing:0.05em; color:#9a94a8; margin-bottom:10px;}
  .lang-toggle{display:flex; justify-content:center; gap:8px; margin-bottom:10px;}
  .lang-btn{
    background:none; border:1px solid #4a4560; color:#9a94a8;
    font-family:'Inter', sans-serif; font-size:12px; letter-spacing:0.05em;
    padding:5px 14px; border-radius:20px; cursor:pointer;
  }
  .lang-btn.active{background:var(--gold); border-color:var(--gold); color:#1a1520;}
  .lang-btn:hover:not(.active){border-color:var(--gold); color:var(--gold);}
  .badge{display:block; text-align:center; font-size:11px; color:var(--gold); letter-spacing:0.05em; margin-bottom:22px;}

  .card{
    background:var(--paper); color:var(--ink-text); border-radius:4px;
    padding:26px 24px; box-shadow:0 30px 60px -20px rgba(0,0,0,0.6), 0 0 0 1px rgba(184,147,90,0.15);
    position:relative;
  }
  .card::before{content:""; position:absolute; inset:8px; border:1px solid rgba(163,56,42,0.2); pointer-events:none;}

  label{display:block; font-size:11px; text-transform:uppercase; letter-spacing:0.1em; color:#6b6153; margin-bottom:6px; font-weight:600;}
  input[type=text]{
    width:100%; font-size:15px; padding:11px 13px; border:1px solid var(--paper-dim);
    background:#f6f1e6; border-radius:2px; color:var(--ink-text); font-family:inherit;
  }
  input[type=text]:focus{outline:2px solid var(--seal); outline-offset:1px;}
  #setupForm label:not(:first-child){margin-top:0;}
  #setupForm > label:first-of-type{margin-top:0;}
  .q-field{margin-bottom:18px;}
  .num-row{display:flex; gap:14px; margin-bottom:20px;}
  .num-field{flex:1;}
  .num-control{display:flex; border:1px solid var(--paper-dim); background:#f6f1e6; border-radius:2px; overflow:hidden;}
  .num-control input{flex:1; border:none; background:transparent; font-family:'JetBrains Mono', monospace; font-size:16px; text-align:center; padding:10px 4px; color:var(--ink-text);}
  .num-control input:focus{outline:none;}
  .num-btn{border:none; background:transparent; color:var(--seal); font-size:16px; width:32px; cursor:pointer;}
  .num-btn:hover{background:rgba(163,56,42,0.08);}
  .dice-btn{border:none; background:transparent; color:#8a8070; width:32px; cursor:pointer; font-size:14px; border-left:1px solid var(--paper-dim);}
  .dice-btn:hover{color:var(--seal);}
  .cast-btn{
    width:100%; background:var(--seal); color:var(--paper); border:none; padding:14px;
    font-family:'Cormorant Garamond', serif; font-size:18px; letter-spacing:0.08em; font-weight:600;
    border-radius:2px; cursor:pointer;
  }
  .cast-btn:hover{background:var(--seal-dark);}
  .cast-btn:disabled{opacity:0.55; cursor:default;}

  .hexline-wrap{display:none; justify-content:center; margin-bottom:16px;}
  .hexlines{display:flex; flex-direction:column-reverse; gap:6px;}
  .hexline{width:100px; height:8px; position:relative;}
  .hexline .bar{position:absolute; top:0; bottom:0; background:var(--ink-text); border-radius:1px;}
  .hexline.solid .bar{left:0; right:0;}
  .hexline.broken .bar1{left:0; width:44%;} .hexline.broken .bar2{right:0; width:44%;}
  .hexline.active .bar{background:var(--seal);}

  .que-title{text-align:center; font-family:'Cormorant Garamond', serif; font-size:22px; font-weight:700; margin-bottom:2px; display:none;}
  .que-sub{text-align:center; font-size:11px; letter-spacing:0.08em; color:#8a8070; text-transform:uppercase; margin-bottom:14px; display:none;}
  .classical-box{
    display:none; font-family:'Cormorant Garamond', serif; font-style:italic; font-size:13.5px; color:#6b6153;
    background:#f6f1e6; border-left:2px solid var(--paper-dim); padding:11px 13px; margin-bottom:18px; line-height:1.45;
  }

  #chatThread{display:none; margin-bottom:16px;}
  .bubble{margin-bottom:14px; display:flex;}
  .bubble.thao{justify-content:flex-start;}
  .bubble.user{justify-content:flex-end;}
  .bubble-inner{
    max-width:88%; padding:12px 14px; border-radius:10px; font-size:14.5px; line-height:1.6;
    white-space:pre-wrap;
  }
  .bubble.thao .bubble-inner{background:#f6f1e6; border:1px solid var(--paper-dim); border-top-left-radius:2px;}
  .bubble.user .bubble-inner{background:var(--seal); color:var(--paper); border-top-right-radius:2px;}
  .bubble-name{font-size:10px; text-transform:uppercase; letter-spacing:0.08em; color:var(--gold); margin-bottom:4px; font-weight:600;}
  .bubble.user .bubble-name{text-align:right; color:#d9a98f;}
  .cursor{display:inline-block; width:7px; height:14px; background:var(--seal); margin-left:2px; animation:blink 0.9s steps(1) infinite; vertical-align:text-bottom;}
  @keyframes blink{50%{opacity:0;}}

  #followupRow{display:none; gap:8px;}
  #followupInput{flex:1;}
  .send-btn{
    background:var(--seal); color:var(--paper); border:none; padding:0 18px; border-radius:2px;
    font-family:'Cormorant Garamond', serif; font-size:16px; font-weight:600; cursor:pointer;
  }
  .send-btn:hover{background:var(--seal-dark);}
  .send-btn:disabled{opacity:0.5; cursor:default;}

  .reset-link{display:none; text-align:center; margin-top:14px;}
  .reset-link button{background:none; border:none; color:#8a8070; font-size:12px; text-decoration:underline; cursor:pointer;}

  .error-box{background:#f6e6e0; border-left:3px solid var(--seal); padding:12px 14px; font-size:13.5px; border-radius:2px; margin-bottom:14px;}

  footer{text-align:center; font-size:11px; color:#4d4860; margin-top:22px; line-height:1.6;}
</style>
</head>
<body>
<div class="wrap">
  <div class="brush">\u5361</div>
  <h1>Thao's Fortune Telling Shop</h1>
  <div class="subtitle" id="subtitleText">a que &middot; hao fortune reading</div>
  <div class="lang-toggle">
    <button class="lang-btn active" id="langEnBtn" onclick="setLang('en')">EN</button>
    <button class="lang-btn" id="langJaBtn" onclick="setLang('ja')">\u65e5\u672c\u8a9e</button>
  </div>
  <div class="badge" id="badgeText">&#10024; live AI reading &middot; ask follow-ups</div>

  <div class="card">
    <div id="setupForm">
      <div class="q-field">
        <label for="question" id="qLabel">Your question</label>
        <input type="text" id="question" placeholder="e.g. Should I take the new job offer, and why does it feel so hard to decide?">
      </div>
      <div class="num-row">
        <div class="num-field">
          <label for="que" id="queLabel">Que (1&ndash;64)</label>
          <div class="num-control">
            <button class="num-btn" onclick="step('que',-1)">&minus;</button>
            <input type="number" id="que" min="1" max="64" value="1">
            <button class="num-btn" onclick="step('que',1)">+</button>
            <button class="dice-btn" title="Random" onclick="randomize('que',64)">&#127922;</button>
          </div>
        </div>
        <div class="num-field">
          <label for="hao" id="haoLabel">Hao (1&ndash;6)</label>
          <div class="num-control">
            <button class="num-btn" onclick="step('hao',-1)">&minus;</button>
            <input type="number" id="hao" min="1" max="6" value="1">
            <button class="num-btn" onclick="step('hao',1)">+</button>
            <button class="dice-btn" title="Random" onclick="randomize('hao',6)">&#127922;</button>
          </div>
        </div>
      </div>
      <button class="cast-btn" id="castBtn" onclick="startReading()">Cast the Reading</button>
    </div>

    <div class="hexline-wrap" id="hexlineWrap"><div class="hexlines" id="hexlines"></div></div>
    <div class="que-title" id="queTitle"></div>
    <div class="que-sub" id="queSub"></div>
    <div class="classical-box" id="classicalBox"></div>

    <div id="errorBox"></div>
    <div id="chatThread"></div>

    <div id="followupRow">
      <input type="text" id="followupInput" placeholder="Ask Thao a follow-up..." onkeydown="if(event.key==='Enter') sendFollowup();">
      <button class="send-btn" id="sendBtn" onclick="sendFollowup()">Ask</button>
    </div>

    <div class="reset-link" id="resetLink">
      <button onclick="resetAll()" id="resetBtn">&larr; start a new reading</button>
    </div>
  </div>

  <footer>
    Thao is played by Claude, reading the classical Judgment and Hao text live and replying in real time.<br>
    Works here in Claude.ai. A public deployed version would need its own backend to call the API safely.
  </footer>
</div>

<script>
// IMPORTANT: replace this with YOUR actual Cloudflare Worker URL from Step 3
const WORKER_URL = "https://thao-fortune-proxy.thao-tran.workers.dev";

const OFFLINE_DATA = {"1": {"english": "Ch'ien / The Creative", "wilhelm_judgment": {"text": "THE CREATIVE works sublime success, furthering through perseverance."}, "wilhelm_image": {"text": "The movement of heaven is full of power. Thus the superior man makes himself strong and untiring."}, "wilhelm_lines": {"1": {"text": "Hidden dragon. Do not act."}, "2": {"text": "Dragon appearing in the field. It furthers one to see the great man."}, "3": {"text": "All day long the superior man is creatively active. At nightfall his mind is still beset with cares. Danger. No blame."}, "4": {"text": "Wavering flight over the depths. No blame."}, "5": {"text": "Flying dragon in the heavens. It furthers one to see the great man."}, "6": {"text": "Arrogant dragon will have cause to repent."}}}, "2": {"english": "K'un / The Receptive", "wilhelm_judgment": {"text": "THE RECEPTIVE brings about sublime success, furthering through the perseverance of a mare. If the superior man undertakes something and tries to lead, he goes astray; but if he follows, he finds guidance."}, "wilhelm_image": {"text": "The earth's condition is receptive devotion. Thus the superior man who has breadth of character carries the outer world."}, "wilhelm_lines": {"1": {"text": "When there is hoarfrost underfoot, solid ice is not far off."}, "2": {"text": "Straight, square, great. Without purpose, yet nothing remains unfurthered."}, "3": {"text": "Hidden lines. One is able to remain persevering. If by chance you are in the service of a king, seek not works, but bring to completion."}, "4": {"text": "A tied-up sack. No blame, no praise."}, "5": {"text": "A yellow lower garment brings supreme good fortune."}, "6": {"text": "Dragons fight in the meadow. Their blood is black and yellow."}}}, "3": {"english": "Chun / Difficulty at the Beginning", "wilhelm_judgment": {"text": "DIFFICULTY AT THE BEGINNING works supreme success, furthering through perseverance. Nothing should be undertaken. It furthers one to appoint helpers."}, "wilhelm_image": {"text": "Clouds and thunder: the image of DIFFICULTY AT THE BEGINNING. Thus the superior man brings order out of confusion."}, "wilhelm_lines": {"1": {"text": "Hesitation and hindrance. It furthers one to remain persevering. It furthers one to appoint helpers."}, "2": {"text": "Difficulties pile up. Horse and wagon part. He is not a robber; he wants to woo when the time comes. Ten years, then she pledges herself."}, "3": {"text": "Whoever hunts deer without the forester only loses his way in the forest. The superior man understands the signs of the time and prefers to desist."}, "4": {"text": "Horse and wagon part. Strive for union. To go brings good fortune. Everything acts to further."}, "5": {"text": "Difficulties in blessing. A little perseverance brings good fortune. Great perseverance brings misfortune."}, "6": {"text": "Horse and wagon part. Bloody tears flow."}}}, "4": {"english": "Mêng / Youthful Folly", "wilhelm_judgment": {"text": "YOUTHFUL FOLLY has success. It is not I who seek the young fool; the young fool seeks me. Perseverance furthers."}, "wilhelm_image": {"text": "A spring wells up at the foot of the mountain: the image of YOUTH. Thus the superior man fosters his character by thoroughness in all that he does."}, "wilhelm_lines": {"1": {"text": "To make a fool develop it furthers one to apply discipline. The fetters should be removed. To go on in this way brings humiliation."}, "2": {"text": "To bear with fools in kindliness brings good fortune. The son is capable of taking charge of the household."}, "3": {"text": "Take not a maiden who, when she sees a man of bronze, loses possession of herself. Nothing furthers."}, "4": {"text": "Entangled folly brings humiliation."}, "5": {"text": "Childlike folly brings good fortune."}, "6": {"text": "In punishing folly it does not further one to commit transgressions. The only thing that furthers is to prevent transgressions."}}}, "5": {"english": "Hsü / Waiting (Nourishment)", "wilhelm_judgment": {"text": "WAITING. If you are sincere, you have light and success. Perseverance brings good fortune. It furthers one to cross the great water."}, "wilhelm_image": {"text": "Clouds rise up to heaven: the image of WAITING. Thus the superior man eats and drinks, is joyous and of good cheer."}, "wilhelm_lines": {"1": {"text": "Waiting in the meadow. It furthers one to abide in what endures. No blame."}, "2": {"text": "Waiting on the sand. There is some gossip. The end brings good fortune."}, "3": {"text": "Waiting in the mud brings about the arrival of the enemy."}, "4": {"text": "Waiting in blood. Get out of the pit."}, "5": {"text": "Waiting at meat and drink. Perseverance brings good fortune."}, "6": {"text": "One falls into the pit. Three uninvited guests arrive. Honor them, and in the end there will be good fortune."}}}, "6": {"english": "Sung / Conflict", "wilhelm_judgment": {"text": "CONFLICT. You are sincere and are being obstructed. A cautious halt halfway brings good fortune. Going through to the end brings misfortune."}, "wilhelm_image": {"text": "Heaven and water go their opposite ways: the image of CONFLICT. Thus in all his transactions the superior man carefully considers the beginning."}, "wilhelm_lines": {"1": {"text": "If one does not perpetuate the affair, there is a little gossip. In the end, good fortune comes."}, "2": {"text": "One cannot engage in conflict; one returns home, gives way. The people of his town remain free of guilt."}, "3": {"text": "To nourish oneself on ancient virtue induces perseverance. Danger. In the end, good fortune comes."}, "4": {"text": "One cannot engage in conflict. One turns back and submits to fate, and finds peace in perseverance. Good fortune."}, "5": {"text": "To contend before him brings supreme good fortune."}, "6": {"text": "Even if by chance a leather belt is bestowed on one, by the end of a morning it will have been snatched away three times."}}}, "7": {"english": "Shih / The Army", "wilhelm_judgment": {"text": "THE ARMY. The army needs perseverance and a strong man. Good fortune without blame."}, "wilhelm_image": {"text": "In the middle of the earth is water: the image of THE ARMY. Thus the superior man increases his masses by generosity toward the people."}, "wilhelm_lines": {"1": {"text": "An army must set forth in proper order. If the order is not good, misfortune threatens."}, "2": {"text": "In the midst of the army. Good fortune. No blame. The king bestows a triple decoration."}, "3": {"text": "Perchance the army carries corpses in the wagon. Misfortune."}, "4": {"text": "The army retreats. No blame."}, "5": {"text": "There is game in the field. It furthers one to catch it. Without blame. Let the eldest lead the army."}, "6": {"text": "The great prince issues commands, founds states, vests families with fiefs. Inferior people should not be employed."}}}, "8": {"english": "Pi / Holding Together", "wilhelm_judgment": {"text": "HOLDING TOGETHER brings good fortune. Those who are uncertain gradually join. Whoever comes too late meets with misfortune."}, "wilhelm_image": {"text": "On the earth is water: the image of HOLDING TOGETHER. Thus the kings of antiquity cultivated friendly relations with the feudal lords."}, "wilhelm_lines": {"1": {"text": "Hold to him in truth and loyalty; this is without blame."}, "2": {"text": "Hold to him inwardly. Perseverance brings good fortune."}, "3": {"text": "You hold together with the wrong people."}, "4": {"text": "Hold to him outwardly also. Perseverance brings good fortune."}, "5": {"text": "Manifestation of holding together. The citizens need no warning. Good fortune."}, "6": {"text": "He finds no head for holding together. Misfortune."}}}, "9": {"english": "Hsiao Ch'u / The Taming Power of the Small", "wilhelm_judgment": {"text": "THE TAMING POWER OF THE SMALL has success. Dense clouds, no rain from our western region."}, "wilhelm_image": {"text": "The wind drives across heaven: the image of THE TAMING POWER OF THE SMALL. Thus the superior man refines the outward aspect of his nature."}, "wilhelm_lines": {"1": {"text": "Return to the way. How could there be blame in this? Good fortune."}, "2": {"text": "He allows himself to be drawn into returning. Good fortune."}, "3": {"text": "The spokes burst out of the wagon wheels. Man and wife roll their eyes."}, "4": {"text": "If you are sincere, blood vanishes and fear gives way. No blame."}, "5": {"text": "If you are sincere and loyally attached, you are rich in your neighbor."}, "6": {"text": "The rain comes, there is rest. This is due to the lasting effect of character. The moon is nearly full."}}}, "10": {"english": "Lü / Treading", "wilhelm_judgment": {"text": "TREADING. Treading upon the tail of the tiger. It does not bite the man. Success."}, "wilhelm_image": {"text": "Heaven above, the lake below: the image of TREADING. Thus the superior man discriminates between high and low."}, "wilhelm_lines": {"1": {"text": "Simple conduct. Progress without blame."}, "2": {"text": "Treading a smooth, level course. The perseverance of a dark man brings good fortune."}, "3": {"text": "A one-eyed man is able to see, a lame man is able to tread. He treads on the tail of the tiger. The tiger bites the man. Misfortune."}, "4": {"text": "He treads on the tail of the tiger. Caution and circumspection lead ultimately to good fortune."}, "5": {"text": "Resolute conduct. Perseverance with awareness of danger."}, "6": {"text": "Look to your conduct and weigh the favorable signs. When everything is fulfilled, supreme good fortune comes."}}}, "11": {"english": "T'ai / Peace", "wilhelm_judgment": {"text": "PEACE. The small departs, the great approaches. Good fortune. Success."}, "wilhelm_image": {"text": "Heaven and earth unite: the image of PEACE. Thus the ruler divides and completes the course of heaven and earth, and so aids the people."}, "wilhelm_lines": {"1": {"text": "When ribbon grass is pulled up, the sod comes with it. Each according to his kind. Undertakings bring good fortune."}, "2": {"text": "Bearing with the uncultured in gentleness, fording the river with resolution. Thus one may manage to walk in the middle."}, "3": {"text": "No plain not followed by a slope. No going not followed by a return. He who remains persevering in danger is without blame."}, "4": {"text": "He flutters down, not boasting of his wealth, together with his neighbor, guileless and sincere."}, "5": {"text": "The sovereign I gives his daughter in marriage. Supreme good fortune."}, "6": {"text": "The wall falls back into the moat. Use no army now. Perseverance brings humiliation."}}}, "12": {"english": "P'i / Standstill", "wilhelm_judgment": {"text": "STANDSTILL. Evil people do not further the perseverance of the superior man. The great departs; the small approaches."}, "wilhelm_image": {"text": "Heaven and earth do not unite: the image of STANDSTILL. Thus the superior man falls back upon his inner worth in order to escape the difficulties."}, "wilhelm_lines": {"1": {"text": "When ribbon grass is pulled up, the sod comes with it. Perseverance brings good fortune and success."}, "2": {"text": "They bear and endure; this means good fortune for inferior people. The standstill serves to help the great man to attain success."}, "3": {"text": "They bear shame."}, "4": {"text": "He who acts at the command of the highest remains without blame. Those of like mind partake of the blessing."}, "5": {"text": "Standstill is giving way. Good fortune for the great man."}, "6": {"text": "The standstill comes to an end. First standstill, then good fortune."}}}, "13": {"english": "T'ung Jên / Fellowship with Men", "wilhelm_judgment": {"text": "FELLOWSHIP WITH MEN in the open. Success. It furthers one to cross the great water. The perseverance of the superior man furthers."}, "wilhelm_image": {"text": "Heaven together with fire: the image of FELLOWSHIP WITH MEN. Thus the superior man organizes the clans and makes distinctions between things."}, "wilhelm_lines": {"1": {"text": "Fellowship with men at the gate. No blame."}, "2": {"text": "Fellowship with men in the clan. Humiliation."}, "3": {"text": "He hides weapons in the thicket; he climbs the high hill in front of it. For three years he does not rise up."}, "4": {"text": "He climbs up on his wall; he cannot attack. Good fortune."}, "5": {"text": "Men bound in fellowship first weep and lament, but afterward they laugh. After great struggles they succeed in meeting."}, "6": {"text": "Fellowship with men in the meadow. No remorse."}}}, "14": {"english": "Ta Yu / Possession in Great Measure", "wilhelm_judgment": {"text": "POSSESSION IN GREAT MEASURE. Supreme success."}, "wilhelm_image": {"text": "Fire in heaven above: the image of POSSESSION IN GREAT MEASURE. Thus the superior man curbs evil and furthers good."}, "wilhelm_lines": {"1": {"text": "No relationship with what is harmful; there is no blame in this. If one remains conscious of difficulty, one remains without blame."}, "2": {"text": "A big wagon for loading. One may undertake something. No blame."}, "3": {"text": "A prince offers it to the Son of Heaven. A petty man cannot do this."}, "4": {"text": "He makes a difference between himself and his neighbor. No blame."}, "5": {"text": "He whose truth is accessible, yet dignified, has good fortune."}, "6": {"text": "He is blessed by heaven. Good fortune. Nothing that does not further."}}}, "15": {"english": "Ch'ien / Modesty", "wilhelm_judgment": {"text": "MODESTY creates success. The superior man carries things through."}, "wilhelm_image": {"text": "Within the earth, a mountain: the image of MODESTY. Thus the superior man reduces that which is too much, and augments that which is too little."}, "wilhelm_lines": {"1": {"text": "A superior man modest about his modesty may cross the great water. Good fortune."}, "2": {"text": "Modesty that comes to expression. Perseverance brings good fortune."}, "3": {"text": "A superior man of modesty and merit carries things to conclusion. Good fortune."}, "4": {"text": "Nothing that would not further modesty in movement."}, "5": {"text": "No boasting of wealth before one's neighbor. It is favorable to attack with force. Nothing that would not further."}, "6": {"text": "Modesty that comes to expression. It is favorable to set armies marching to chastise one's own city and one's country."}}}, "16": {"english": "Yü / Enthusiasm", "wilhelm_judgment": {"text": "ENTHUSIASM. It furthers one to install helpers and to set armies marching."}, "wilhelm_image": {"text": "Thunder comes resounding out of the earth: the image of ENTHUSIASM. Thus the ancient kings made music in order to honor merit."}, "wilhelm_lines": {"1": {"text": "Enthusiasm that expresses itself brings misfortune."}, "2": {"text": "Firm as a rock. Not a whole day. Perseverance brings good fortune."}, "3": {"text": "Enthusiasm that looks upward creates remorse. Hesitation brings remorse."}, "4": {"text": "The source of enthusiasm. He achieves great things. Doubt not. You gather friends around you."}, "5": {"text": "Persistently ill, and still does not die."}, "6": {"text": "Deluded enthusiasm. But if after completion one changes, there is no blame."}}}, "17": {"english": "Sui / Following", "wilhelm_judgment": {"text": "FOLLOWING has supreme success. Perseverance furthers. No blame."}, "wilhelm_image": {"text": "Thunder in the middle of the lake: the image of FOLLOWING. Thus the superior man at nightfall goes indoors for rest and recuperation."}, "wilhelm_lines": {"1": {"text": "The standard is changing. Perseverance brings good fortune. To go out of the door in company produces deeds."}, "2": {"text": "If one clings to the little boy, one loses the strong man."}, "3": {"text": "If one clings to the strong man, one loses the little boy. Through following one finds what one seeks."}, "4": {"text": "Following creates success. Perseverance brings misfortune. To go one's way with sincerity brings clarity."}, "5": {"text": "Sincere in the good. Good fortune."}, "6": {"text": "He meets with firm allegiance and is still further bound. The king introduces him to the Western Mountain."}}}, "18": {"english": "Ku / Work on the Decayed", "wilhelm_judgment": {"text": "What has been spoiled through neglect can be set right through decisive work. Supreme success comes through careful correction of old mistakes."}, "wilhelm_image": {"text": "Wind at the foot of the mountain: decay lies still until stirred. The wise person rouses others and strengthens character through corrective action."}, "wilhelm_lines": {"1": {"text": "Correcting a parent's fault takes courage; approached with respect, it succeeds."}, "2": {"text": "Correcting gently, not too severely, avoids future regret."}, "3": {"text": "Correcting too sharply brings small remorse, but no lasting blame."}, "4": {"text": "Leaving the decay untouched brings humiliation."}, "5": {"text": "Correcting with honor and praise wins recognition."}, "6": {"text": "Stepping beyond worldly service toward higher aims is honorable."}}}, "19": {"english": "Lin / Approach", "wilhelm_judgment": {"text": "Approach brings supreme success. Perseverance furthers, though as growth peaks, caution against overreach is wise."}, "wilhelm_image": {"text": "Earth above the lake: the superior man's teaching and care are without limit."}, "wilhelm_lines": {"1": {"text": "Approaching together in sincerity brings good fortune."}, "2": {"text": "Approaching together in sincerity: good fortune, nothing that does not further."}, "3": {"text": "Complacent approach brings no benefit, but honest concern removes blame."}, "4": {"text": "Approaching to the utmost, in true devotion, brings no blame."}, "5": {"text": "Wise delegation in approach brings great good fortune."}, "6": {"text": "Generous, sincere approach brings good fortune, without blame."}}}, "20": {"english": "Kuan / Contemplation", "wilhelm_judgment": {"text": "Contemplation: like a ritual not yet completed, truthfulness commands quiet respect."}, "wilhelm_image": {"text": "Wind moves above the earth: the wise observe the people and set an example through teaching."}, "wilhelm_lines": {"1": {"text": "A narrow, boyish view: no blame for the ordinary, but humbling for one who should know better."}, "2": {"text": "Contemplation through a narrow crack limits understanding."}, "3": {"text": "Contemplating one's own life guides whether to advance or withdraw."}, "4": {"text": "Contemplating the light of the kingdom favors seeking honored service."}, "5": {"text": "Contemplating one's own life as a superior person brings no blame."}, "6": {"text": "Contemplating one's character with detachment brings no blame."}}}, "21": {"english": "Shih Ho / Biting Through", "wilhelm_judgment": {"text": "Biting through brings success. It furthers one to let justice be administered clearly."}, "wilhelm_image": {"text": "Thunder and lightning: the wise frame clear penalties and enforce the laws."}, "wilhelm_lines": {"1": {"text": "A small early check, firmly applied, prevents a worse fault later."}, "2": {"text": "Biting through tender meat, going a bit too far, but no blame."}, "3": {"text": "Biting on old, tough meat meets resistance; slight humiliation, no great blame."}, "4": {"text": "Difficulty overcome through perseverance and awareness proves favorable."}, "5": {"text": "Persevering and aware of danger, one meets with no blame."}, "6": {"text": "Ignoring warnings too long invites real misfortune."}}}, "22": {"english": "Pi / Grace", "wilhelm_judgment": {"text": "Grace brings success in small matters. It furthers one to undertake something, in modest ways."}, "wilhelm_image": {"text": "Fire at the foot of the mountain: the wise keep judgments clear and avoid needless delay."}, "wilhelm_lines": {"1": {"text": "Choosing dignity and simplicity over show and speed."}, "2": {"text": "Adorning what already belongs to oneself, rather than reaching for more."}, "3": {"text": "Graceful and at ease: perseverance brings continuing good fortune."}, "4": {"text": "Seeking union rather than advantage brings the right result."}, "5": {"text": "Modest gifts, offered sincerely, bring good fortune in the end."}, "6": {"text": "Simple, unadorned grace: no blame."}}}, "23": {"english": "Po / Splitting Apart", "wilhelm_judgment": {"text": "Splitting apart: it does not further one to go anywhere right now."}, "wilhelm_image": {"text": "The mountain rests on the earth: those above secure their position through generosity below."}, "wilhelm_lines": {"1": {"text": "The foundation is being undermined; perseverance alone brings misfortune."}, "2": {"text": "The undermining continues; caution is needed."}, "3": {"text": "Splitting apart, yet staying connected to what is right: no blame."}, "4": {"text": "Danger draws very near."}, "5": {"text": "Favor granted through those close by: nothing that does not further."}, "6": {"text": "The superior man is carried onward even as the old structure gives way."}}}, "24": {"english": "Fu / Return", "wilhelm_judgment": {"text": "Return brings success. Coming and going without error; the way returns in its own time."}, "wilhelm_image": {"text": "Thunder within the earth: the ancients rested at the turning point and allowed renewal."}, "wilhelm_lines": {"1": {"text": "A short return, caught early: no need for remorse, great good fortune."}, "2": {"text": "A quiet, unassuming return: good fortune."}, "3": {"text": "A repeated, uneasy return: no blame in the end."}, "4": {"text": "Walking one's own path back to what is right."}, "5": {"text": "A noble-minded return: no cause for remorse."}, "6": {"text": "Missing the moment to return invites misfortune."}}}, "25": {"english": "Wu Wang / Innocence", "wilhelm_judgment": {"text": "Innocence brings supreme success. Perseverance furthers; acting out of harmony with what is right brings misfortune."}, "wilhelm_image": {"text": "Under heaven, thunder rolls: all things attain their natural, innocent course."}, "wilhelm_lines": {"1": {"text": "Innocent, unforced action brings good fortune."}, "2": {"text": "Acting without ulterior motive brings its own reward."}, "3": {"text": "Unexpected loss touches those without blame."}, "4": {"text": "Remaining firm and true meets no blame."}, "5": {"text": "An unexpected trouble resolves itself without forcing a cure."}, "6": {"text": "Innocent action pushed too far invites error; better to pause."}}}, "26": {"english": "Ta Ch'u / The Taming Power of the Great", "wilhelm_judgment": {"text": "Great taming power: perseverance furthers. It furthers one to cross the great water."}, "wilhelm_image": {"text": "Heaven within the mountain: the wise study the past to develop their character."}, "wilhelm_lines": {"1": {"text": "Danger is at hand; it is better to hold back."}, "2": {"text": "Restraint now avoids trouble later."}, "3": {"text": "Awareness of difficulty, paired with steady practice, furthers."}, "4": {"text": "Gentle, early restraint brings great good fortune."}, "5": {"text": "Good fortune comes through restraint applied at the source."}, "6": {"text": "One attains the open way: success."}}}, "27": {"english": "I / Nourishment", "wilhelm_judgment": {"text": "Nourishment: perseverance brings good fortune. Pay attention to what truly nourishes, and to what you seek to fill yourself with."}, "wilhelm_image": {"text": "Thunder at the foot of the mountain: the wise are careful of their words and temperate in what they consume."}, "wilhelm_lines": {"1": {"text": "Neglecting one's own resources while envying others' brings misfortune."}, "2": {"text": "Turning to the wrong source for nourishment, departing from the right path, brings misfortune."}, "3": {"text": "Turning away from proper nourishment: perseverance brings misfortune for now."}, "4": {"text": "Seeking help from above, with watchful care, brings good fortune."}, "5": {"text": "Remaining persevering, even while relying on others, brings good fortune."}, "6": {"text": "Awareness of the source of true nourishment brings good fortune."}}}, "28": {"english": "Ta Kuo / Preponderance of the Great", "wilhelm_judgment": {"text": "Great preponderance: the ridgepole sags. It furthers one to act; success is possible."}, "wilhelm_image": {"text": "The lake rises above the trees: the superior man stands firm, even alone, and accepts what must be renounced."}, "wilhelm_lines": {"1": {"text": "Extreme caution at the outset avoids blame."}, "2": {"text": "An unlikely new beginning still brings benefit."}, "3": {"text": "Pushed to the breaking point: misfortune."}, "4": {"text": "Bracing the structure in time brings good fortune, despite strain."}, "5": {"text": "A late, unusual flourishing brings no blame, no praise."}, "6": {"text": "In over one's head: misfortune, but no blame for trying."}}}, "29": {"english": "K'an / The Abysmal (Water)", "wilhelm_judgment": {"text": "The Abysmal repeated: through sincerity, the heart succeeds; sincere action brings recognition."}, "wilhelm_image": {"text": "Water flows on without ceasing: the superior man walks steadily in virtue and carries on the work of teaching."}, "wilhelm_lines": {"1": {"text": "Falling repeatedly into the same pit: misfortune."}, "2": {"text": "Facing danger, seeking only small, safe gains for now."}, "3": {"text": "Coming and going amid danger; better to pause than to act."}, "4": {"text": "Simple sincerity, plainly offered, brings no blame in the end."}, "5": {"text": "Steadiness alone, even without full resolution, brings no blame."}, "6": {"text": "Caught and bound for a long while: misfortune, but not permanent."}}}, "30": {"english": "Li / The Clinging (Fire)", "wilhelm_judgment": {"text": "The Clinging: perseverance furthers, and brings success. Careful, steady effort brings good fortune."}, "wilhelm_image": {"text": "Light doubled: the great person perpetuates brightness, illuminating what is around them."}, "wilhelm_lines": {"1": {"text": "Hesitant steps at the start; care avoids blame."}, "2": {"text": "Steady, balanced light: supreme good fortune."}, "3": {"text": "Facing an ending with grace, rather than lament, avoids misfortune."}, "4": {"text": "A sudden flare that dies down quickly: instability without lasting foundation."}, "5": {"text": "Sincere grief, openly expressed, leads to good fortune."}, "6": {"text": "Correcting wrongdoing decisively brings good fortune, without blame."}}}, "31": {"english": "Hsien / Influence", "wilhelm_judgment": {"text": "Influence: success. Perseverance furthers, especially in matters of the heart."}, "wilhelm_image": {"text": "A lake on the mountain: the superior man welcomes others with an open, receptive spirit."}, "wilhelm_lines": {"1": {"text": "Influence just beginning: no significant movement yet."}, "2": {"text": "Staying still, rather than moving rashly, brings good fortune."}, "3": {"text": "Following without independence brings humiliation."}, "4": {"text": "Constant, settled influence brings good fortune; remorse disappears."}, "5": {"text": "Quiet, steady influence brings no remorse."}, "6": {"text": "Mere talk without substance carries little real influence."}}}, "32": {"english": "Heng / Duration", "wilhelm_judgment": {"text": "Duration: success, no blame. Perseverance furthers; it furthers one to have somewhere to go."}, "wilhelm_image": {"text": "Thunder and wind: the superior man stands firm and does not change direction on a whim."}, "wilhelm_lines": {"1": {"text": "Seeking duration too hastily: perseverance brings misfortune for now."}, "2": {"text": "Remorse disappears through steady moderation."}, "3": {"text": "Inconstancy of character brings disgrace."}, "4": {"text": "Effort spent in the wrong direction yields little."}, "5": {"text": "Steadiness suits some situations more than others; know which is which."}, "6": {"text": "Restlessness mistaken for duration brings misfortune."}}}, "33": {"english": "Tun / Retreat", "wilhelm_judgment": {"text": "Retreat: success. In what is small, perseverance furthers."}, "wilhelm_image": {"text": "Mountain beneath heaven: the superior man keeps the unhelpful at a distance, without hatred, but with reserve."}, "wilhelm_lines": {"1": {"text": "Retreating too late brings danger; better not to act rashly."}, "2": {"text": "Firm resolve to withdraw, held with care."}, "3": {"text": "A halted retreat brings danger and unease."}, "4": {"text": "A voluntary, timely retreat brings good fortune."}, "5": {"text": "A friendly, well-timed retreat: perseverance brings good fortune."}, "6": {"text": "A cheerful, unburdened retreat: everything serves to further."}}}, "34": {"english": "Ta Chuang / The Power of the Great", "wilhelm_judgment": {"text": "The power of the great: perseverance furthers, but power must be used rightly."}, "wilhelm_image": {"text": "Thunder in heaven above: the superior man does not tread on paths that are not right."}, "wilhelm_lines": {"1": {"text": "Pressing forward with raw power alone brings misfortune."}, "2": {"text": "Perseverance, tempered with restraint, brings good fortune."}, "3": {"text": "Using power without wisdom leads to entanglement."}, "4": {"text": "Perseverance brings good fortune; remorse disappears."}, "5": {"text": "Letting go of a losing position with ease: no remorse."}, "6": {"text": "Caught between advancing and retreating; care resolves the difficulty."}}}, "35": {"english": "Chin / Progress", "wilhelm_judgment": {"text": "Progress: like a powerful figure honored and repeatedly received; a favorable time to advance."}, "wilhelm_image": {"text": "The sun rises over the earth: the superior man brightens his own bright virtue."}, "wilhelm_lines": {"1": {"text": "Progressing but turned back for now: perseverance brings good fortune."}, "2": {"text": "Progressing through some sorrow; perseverance brings good fortune."}, "3": {"text": "General trust removes remorse."}, "4": {"text": "Progress driven by anxious grasping brings danger."}, "5": {"text": "Letting go of gain and loss brings good fortune."}, "6": {"text": "Pushing forward too forcefully should be reserved for correcting oneself, not others."}}}, "36": {"english": "Ming I / Darkening of the Light", "wilhelm_judgment": {"text": "Darkening of the light: it furthers one to be persevering in adversity."}, "wilhelm_image": {"text": "The light has sunk into the earth: the superior man conceals his brightness while remaining inwardly clear."}, "wilhelm_lines": {"1": {"text": "Withdrawing quietly rather than fighting a losing battle."}, "2": {"text": "Help arrives even amid difficulty; good fortune."}, "3": {"text": "Acting too hastily against a hidden danger brings trouble; patience is wiser."}, "4": {"text": "Gaining the true meaning of a situation from within."}, "5": {"text": "Perseverance in adversity, held with integrity, furthers."}, "6": {"text": "A steep fall follows a high rise when the light is fully lost."}}}, "37": {"english": "Chia Jen / The Family", "wilhelm_judgment": {"text": "The family: perseverance and steady devotion further."}, "wilhelm_image": {"text": "Wind comes forth from fire: the superior man's words have substance, and his conduct has constancy."}, "wilhelm_lines": {"1": {"text": "Firm boundaries within the family, set early, prevent later remorse."}, "2": {"text": "Quiet, steady care, without seeking credit, brings good fortune."}, "3": {"text": "Too much strictness brings regret; too much laxity brings humiliation - balance is needed."}, "4": {"text": "A well-managed household brings great good fortune."}, "5": {"text": "Approaching loved ones with warmth brings good fortune."}, "6": {"text": "Sincerity and dignity, sustained to the end, bring good fortune."}}}, "38": {"english": "K'uei / Opposition", "wilhelm_judgment": {"text": "Opposition: in small matters, good fortune is still possible."}, "wilhelm_image": {"text": "Fire above, lake below, moving in opposite directions: the superior man seeks unity while allowing for individuality."}, "wilhelm_lines": {"1": {"text": "Remorse disappears; something lost returns of itself, without chasing it."}, "2": {"text": "An unplanned meeting in an unlikely place: no blame."}, "3": {"text": "A difficult start can still lead to a good ending."}, "4": {"text": "Isolation eases when a like-minded person is found; caution still furthers."}, "5": {"text": "Trust from someone close removes remorse."}, "6": {"text": "Initial suspicion gives way to understanding as things become clear."}}}, "39": {"english": "Chien / Obstruction", "wilhelm_judgment": {"text": "Obstruction: it furthers one to see the great man; perseverance brings good fortune."}, "wilhelm_image": {"text": "Water on the mountain: the superior man turns his attention within to develop his character."}, "wilhelm_lines": {"1": {"text": "Going forward leads to obstruction; staying brings praise."}, "2": {"text": "Facing obstruction again and again, not for one's own sake."}, "3": {"text": "Going leads to obstruction; returning means safety."}, "4": {"text": "Going leads to obstruction; coming back leads to union with allies."}, "5": {"text": "In the midst of great obstruction, friends come to help."}, "6": {"text": "Going leads to obstruction, but returning meets great good fortune."}}}, "40": {"english": "Hsieh / Deliverance", "wilhelm_judgment": {"text": "Deliverance: if there is nowhere to go, return brings good fortune; if there is somewhere to go, acting soon brings good fortune."}, "wilhelm_image": {"text": "Thunder and rain set in: the superior man pardons mistakes and forgives past misdeeds."}, "wilhelm_lines": {"1": {"text": "Staying still at the turning point between danger and safety: no blame."}, "2": {"text": "Steady effort catches what was chasing you; perseverance brings good fortune."}, "3": {"text": "Carrying too much while moving too fast invites trouble."}, "4": {"text": "Deliverance from what hampers you opens the way for trust to return."}, "5": {"text": "Decisive deliverance from a burden brings good fortune."}, "6": {"text": "Removing a persistent obstacle brings nothing but gain."}}}, "41": {"english": "Sun / Decrease", "wilhelm_judgment": {"text": "Decrease, approached with sincerity, brings supreme good fortune and no blame; it furthers one to undertake something."}, "wilhelm_image": {"text": "At the foot of the mountain, a lake: the superior man controls his anger and restrains his instincts."}, "wilhelm_lines": {"1": {"text": "Offering quick, sincere help, with care not to overextend, brings no blame."}, "2": {"text": "Giving of oneself without loss of steadiness still brings benefit to others."}, "3": {"text": "A smaller group, focused and aligned, accomplishes more than a scattered one."}, "4": {"text": "Decreasing one's own faults brings joy and no blame."}, "5": {"text": "Unexpected support arrives that cannot easily be refused: supreme good fortune."}, "6": {"text": "Giving increase without loss to oneself: no blame, perseverance furthers."}}}, "42": {"english": "I / Increase", "wilhelm_judgment": {"text": "Increase: it furthers one to undertake something; it furthers one to cross the great water."}, "wilhelm_image": {"text": "Wind and thunder: the superior man turns toward good when he sees it, and corrects his faults when aware of them."}, "wilhelm_lines": {"1": {"text": "Great deeds attempted now bring supreme good fortune, no blame."}, "2": {"text": "Unexpected support, offered sincerely, brings good fortune."}, "3": {"text": "Even difficult events can enrich, if met with sincerity."}, "4": {"text": "A balanced, reported approach makes an important move favorable."}, "5": {"text": "A sincere, kindly heart brings supreme good fortune."}, "6": {"text": "Withholding generosity from others, or facing opposition, brings misfortune."}}}, "43": {"english": "Kuai / Breakthrough", "wilhelm_judgment": {"text": "Breakthrough: resolute, but proclaimed openly rather than forced; not favorable to resort to force."}, "wilhelm_image": {"text": "The lake rises to heaven: the superior man shares what he has and does not rest on past achievement."}, "wilhelm_lines": {"1": {"text": "Pushing forward before one is ready to prevail brings blame."}, "2": {"text": "Staying alert, even without immediate cause, avoids trouble."}, "3": {"text": "Standing firm alone, even facing disapproval, remains ultimately blameless."}, "4": {"text": "Difficulty in moving forward eases once advice is heeded."}, "5": {"text": "Firm resolve, balanced and moderate, brings no blame."}, "6": {"text": "Delaying too long without a clear stand brings misfortune."}}}, "44": {"english": "Kou / Coming to Meet", "wilhelm_judgment": {"text": "Coming to meet: caution is needed before committing further."}, "wilhelm_image": {"text": "Under heaven, wind: the wise share their intentions openly and broadly."}, "wilhelm_lines": {"1": {"text": "Checking a rash impulse early: perseverance brings good fortune."}, "2": {"text": "Holding something in reserve, without offering it too soon, brings no blame."}, "3": {"text": "Moving forward with difficulty: danger, but no great blame."}, "4": {"text": "Missing an opportunity through hesitation leads to misfortune."}, "5": {"text": "A hidden strength, held with humility, draws good fortune unexpectedly."}, "6": {"text": "An overly proud stance invites humiliation, but no lasting blame."}}}, "45": {"english": "Ts'ui / Gathering Together", "wilhelm_judgment": {"text": "Gathering together: success. It furthers one to see the great man; perseverance furthers."}, "wilhelm_image": {"text": "The lake rises above the earth: the superior man prepares against the unforeseen."}, "wilhelm_lines": {"1": {"text": "Uncertainty at first gives way to reassurance; no cause for worry."}, "2": {"text": "Being drawn along by trusted others brings good fortune."}, "3": {"text": "Gathering amid some hesitation still brings progress, though not without minor humiliation."}, "4": {"text": "Great good fortune, no blame."}, "5": {"text": "Gathering together in a position of trust: no blame."}, "6": {"text": "Lingering regret fades without lasting blame."}}}, "46": {"english": "Sheng / Pushing Upward", "wilhelm_judgment": {"text": "Pushing upward has supreme success. Advancing steadily brings good fortune."}, "wilhelm_image": {"text": "Within the earth, a tree grows: the superior man cultivates virtue with care, gathering small things to build toward greatness."}, "wilhelm_lines": {"1": {"text": "Pushing upward with confidence meets with great good fortune."}, "2": {"text": "Sincerity brings good fortune, even from modest beginnings."}, "3": {"text": "Pushing upward into open space: unimpeded progress."}, "4": {"text": "A well-timed offering of effort brings good fortune, no blame."}, "5": {"text": "Perseverance brings good fortune, pushing upward step by step."}, "6": {"text": "Pushing upward without pause calls for steady, unceasing perseverance."}}}, "47": {"english": "K'un / Oppression", "wilhelm_judgment": {"text": "Oppression: success is still possible through perseverance; the great man finds good fortune even here."}, "wilhelm_image": {"text": "There is no water in the lake: the superior man holds to his purpose despite hardship."}, "wilhelm_lines": {"1": {"text": "Stuck in a difficult place for a long while, without immediate relief."}, "2": {"text": "Oppressed even amid comfort; caution furthers, undertakings still bring misfortune for now."}, "3": {"text": "Leaning on unstable support in hard times leads to misfortune."}, "4": {"text": "Slow, humbling progress still leads to a good end."}, "5": {"text": "Deliverance from oppression comes slowly, but comes."}, "6": {"text": "Persistent unease resolves into good fortune once one finally acts."}}}, "48": {"english": "Ching / The Well", "wilhelm_judgment": {"text": "The well: circumstances may change, but the underlying source does not; all draw from it."}, "wilhelm_image": {"text": "Water above wood: the superior man encourages people at their work and exhorts them to help one another."}, "wilhelm_lines": {"1": {"text": "A neglected source serves no one; it must be tended."}, "2": {"text": "Potential leaking away through carelessness."}, "3": {"text": "A good resource left unused is a source of quiet grief."}, "4": {"text": "Repairing and reinforcing the source: no blame."}, "5": {"text": "A clear, reliable source is ready to be drawn from."}, "6": {"text": "An open, unguarded source, freely shared, brings supreme good fortune."}}}, "49": {"english": "Ko / Revolution", "wilhelm_judgment": {"text": "Revolution, once the time is truly right, carries conviction; supreme success, perseverance furthers."}, "wilhelm_image": {"text": "Fire in the lake: the superior man sets things in order and makes what is unclear, clear."}, "wilhelm_lines": {"1": {"text": "It is too soon to act; hold back for now."}, "2": {"text": "When the moment is truly right, change becomes possible; good fortune, no blame."}, "3": {"text": "Acting too soon brings danger; repeated confirmation earns trust."}, "4": {"text": "Once trust is established, real change brings good fortune."}, "5": {"text": "Decisive, well-timed change is believed and respected."}, "6": {"text": "Persevering in what is right, even amid change, brings good fortune."}}}, "50": {"english": "Ting / The Cauldron", "wilhelm_judgment": {"text": "The cauldron: supreme good fortune, success."}, "wilhelm_image": {"text": "Fire over wood: the superior man consolidates his position by making it correct."}, "wilhelm_lines": {"1": {"text": "Clearing out what is stagnant, even awkwardly, brings no blame."}, "2": {"text": "Solid substance draws envy, but cannot easily be approached: good fortune."}, "3": {"text": "A blocked path gradually clears as circumstances shift."}, "4": {"text": "Overreaching beyond one's capacity leads to a costly failure."}, "5": {"text": "Reliable, well-placed support furthers."}, "6": {"text": "Refined and strong: great good fortune, nothing that does not further."}}}, "51": {"english": "Chen / The Arousing (Thunder)", "wilhelm_judgment": {"text": "Shock brings success. Apprehension gives way to laughing words; composure holds even amid sudden change."}, "wilhelm_image": {"text": "Thunder repeated: the superior man reflects and stands in awe, setting his conduct in order."}, "wilhelm_lines": {"1": {"text": "An initial shock, met with composure, leads to good fortune."}, "2": {"text": "A real loss follows sudden change, but what's lost returns in time."}, "3": {"text": "Distress that spurs action helps avoid a worse misfortune."}, "4": {"text": "Caught off guard: caution is needed before moving further."}, "5": {"text": "Shock comes and goes; nothing essential is truly lost."}, "6": {"text": "Overreaction to a shock that wasn't even one's own invites gossip."}}}, "52": {"english": "Ken / Keeping Still (Mountain)", "wilhelm_judgment": {"text": "Keeping still: stilling the self so completely that one is at peace, without blame."}, "wilhelm_image": {"text": "Mountains standing close together: the superior man does not let his thoughts wander beyond his own situation."}, "wilhelm_lines": {"1": {"text": "Staying still from the very start: perseverance furthers."}, "2": {"text": "Held back, unable to help someone one wishes to: some discontent."}, "3": {"text": "Forcing stillness under tension is dangerous, not restful."}, "4": {"text": "A calm, settled center: no blame."}, "5": {"text": "Careful, well-ordered words dissolve old remorse."}, "6": {"text": "Genuine, noble-hearted stillness brings good fortune."}}}, "53": {"english": "Chien / Development (Gradual Progress)", "wilhelm_judgment": {"text": "Development: like a marriage well made, good fortune comes through perseverance and gradual progress."}, "wilhelm_image": {"text": "A tree on the mountain: the superior man improves his character and his community little by little."}, "wilhelm_lines": {"1": {"text": "Early steps bring some uncertainty and gossip, but no real blame."}, "2": {"text": "Steady, gradual progress toward safety and peace: good fortune."}, "3": {"text": "Rushing ahead of the natural pace invites misfortune."}, "4": {"text": "Finding a stable, if modest, place to rest: no blame."}, "5": {"text": "A long wait finally resolves into good fortune."}, "6": {"text": "Reaching a high, visible position brings good fortune and respect."}}}, "54": {"english": "Kuei Mei / The Marrying Maiden", "wilhelm_judgment": {"text": "The marrying maiden: acting rashly here brings misfortune; nothing that would further."}, "wilhelm_image": {"text": "Thunder over the lake: the superior man understands the transitory in light of what truly lasts."}, "wilhelm_lines": {"1": {"text": "Accepting a modest, secondary role can still bring good fortune."}, "2": {"text": "Limited vision does not prevent steady, persevering progress."}, "3": {"text": "Settling for less than what is right brings an unsatisfying result."}, "4": {"text": "A delayed outcome, met patiently, arrives at its proper time."}, "5": {"text": "Valuing substance over appearance brings good fortune."}, "6": {"text": "An empty gesture, without real substance behind it, brings nothing that furthers."}}}, "55": {"english": "Feng / Abundance", "wilhelm_judgment": {"text": "Abundance has success; approach it without worry, like the sun at midday."}, "wilhelm_image": {"text": "Thunder and lightning both come: the superior man handles disputes decisively and fairly."}, "wilhelm_lines": {"1": {"text": "Meeting one's equal brings no blame; moving forward earns recognition."}, "2": {"text": "Even in a bright moment, mistrust may arise; sincerity resolves it."}, "3": {"text": "A period of confusion clears once a decisive step is taken."}, "4": {"text": "Meeting the right ally at the right moment brings good fortune."}, "5": {"text": "Bringing capable people together draws blessing and recognition."}, "6": {"text": "Isolating oneself in abundance, cut off from others, ends in misfortune."}}}, "56": {"english": "Lü / The Wanderer", "wilhelm_judgment": {"text": "The wanderer has success in small matters; perseverance brings good fortune to one who travels light."}, "wilhelm_image": {"text": "Fire on the mountain: the superior man is careful and reserved, avoiding unnecessary conflict."}, "wilhelm_lines": {"1": {"text": "Preoccupation with petty things while wandering brings trouble upon oneself."}, "2": {"text": "Finding a place to rest and gaining the trust of those nearby."}, "3": {"text": "Carelessness with what shelters you leads to real loss."}, "4": {"text": "Finding practical means, though not yet fully at ease."}, "5": {"text": "An early setback still ends in praise and recognition."}, "6": {"text": "Carelessness at the height of good fortune can undo everything gained."}}}, "57": {"english": "Sun / The Gentle (Wind)", "wilhelm_judgment": {"text": "The gentle: success through what is small; it furthers one to have somewhere to go."}, "wilhelm_image": {"text": "Winds following one upon another: the superior man spreads his intentions and follows through steadily."}, "wilhelm_lines": {"1": {"text": "Hesitating between advancing and retreating calls for firm resolve."}, "2": {"text": "Thorough, careful investigation brings good fortune, no blame."}, "3": {"text": "Repeating the same uncertain approach brings humiliation."}, "4": {"text": "Remorse disappears once real progress is made."}, "5": {"text": "Careful reconsideration, followed by action, brings good fortune."}, "6": {"text": "Overextending resources under pressure brings misfortune."}}}, "58": {"english": "Tui / The Joyous (Lake)", "wilhelm_judgment": {"text": "The joyous: success. Perseverance furthers when joy is genuine."}, "wilhelm_image": {"text": "Lakes resting one upon the other: the superior man joins with friends for discussion and shared growth."}, "wilhelm_lines": {"1": {"text": "Simple, contented joy: good fortune."}, "2": {"text": "Sincere joy, free of ulterior motive: good fortune, remorse disappears."}, "3": {"text": "Joy sought from outside oneself, rather than within, brings misfortune."}, "4": {"text": "Weighing a decision carefully, even under some unease, leads to a good outcome."}, "5": {"text": "Trusting what is uncertain, with eyes open, is itself a form of strength."}, "6": {"text": "Seductive, easy joy offers no clear answer either way - stay alert."}}}, "59": {"english": "Huan / Dispersion", "wilhelm_judgment": {"text": "Dispersion: success. It furthers one to cross the great water; perseverance furthers."}, "wilhelm_image": {"text": "Wind drives over the water: the wise bring people together around a shared purpose."}, "wilhelm_lines": {"1": {"text": "Offering timely help, with real strength behind it: good fortune."}, "2": {"text": "Hurrying to support what is scattering removes remorse."}, "3": {"text": "Setting aside self-interest for the sake of the whole: no cause for remorse."}, "4": {"text": "Dispersing an old, rigid grouping brings supreme good fortune in the end."}, "5": {"text": "A clear, decisive announcement, sincerely made: no blame."}, "6": {"text": "Distancing oneself from old injury and moving on: no blame."}}}, "60": {"english": "Chieh / Limitation", "wilhelm_judgment": {"text": "Limitation has success, but limits held too harshly should not be persisted in."}, "wilhelm_image": {"text": "Water over the lake: the superior man sets reasonable standards and measures."}, "wilhelm_lines": {"1": {"text": "Staying close to home for now: no blame."}, "2": {"text": "Missing a good opportunity through excessive caution brings misfortune."}, "3": {"text": "Failing to set any limits at all leads to regret."}, "4": {"text": "Comfortable, well-fitted limitation: success."}, "5": {"text": "Limitation applied with kindness brings good fortune and respect."}, "6": {"text": "Limits held too harshly for too long bring misfortune, though remorse eventually eases."}}}, "61": {"english": "Chung Fu / Inner Truth", "wilhelm_judgment": {"text": "Inner truth: sincerity that reaches even the simplest creatures. It furthers one to cross the great water; perseverance furthers."}, "wilhelm_image": {"text": "Wind over the lake: the wise consider matters carefully before acting decisively."}, "wilhelm_lines": {"1": {"text": "Being genuinely prepared brings good fortune; hidden motives disturb one's peace."}, "2": {"text": "A call answered in kind, shared openly with another: good fortune."}, "3": {"text": "Emotions rising and falling with circumstance, rather than steady conviction."}, "4": {"text": "Letting go of an old attachment, at the right moment, brings no blame."}, "5": {"text": "Sincerity that binds people together: no blame."}, "6": {"text": "Confidence voiced too loudly, without real substance, can bring misfortune."}}}, "62": {"english": "Hsiao Kuo / Preponderance of the Small", "wilhelm_judgment": {"text": "Preponderance of the small: success in small matters; better to remain modest than to strive upward."}, "wilhelm_image": {"text": "Thunder on the mountain: the superior man is scrupulous in small things and careful not to overreach."}, "wilhelm_lines": {"1": {"text": "A rash, premature move brings trouble upon oneself."}, "2": {"text": "Meeting an unexpected, indirect helper: no blame."}, "3": {"text": "Failing to guard against a small danger invites misfortune."}, "4": {"text": "Caution, rather than bold advance, avoids blame right now."}, "5": {"text": "Conditions aren't quite ready; patience is called for."}, "6": {"text": "Overreaching past a natural limit brings misfortune from more than one direction."}}}, "63": {"english": "Chi Chi / After Completion", "wilhelm_judgment": {"text": "After completion: success in small matters. Perseverance furthers, but vigilance is needed even after things settle."}, "wilhelm_image": {"text": "Water over fire: the superior man anticipates trouble and guards against it in advance."}, "wilhelm_lines": {"1": {"text": "Holding back a little at the very end avoids blame."}, "2": {"text": "A temporary loss returns of its own accord if not chased."}, "3": {"text": "A hard-won victory should not lead to overconfidence afterward."}, "4": {"text": "Even fine achievements need ongoing care; caution all day long."}, "5": {"text": "Genuine, modest effort brings more real benefit than showy excess."}, "6": {"text": "Pushing on past the point of completion brings unnecessary danger."}}}, "64": {"english": "Wei Chi / Before Completion", "wilhelm_judgment": {"text": "Before completion: success is possible, but rushing the final step undoes the gain."}, "wilhelm_image": {"text": "Fire over water: the superior man is careful in distinguishing things, so each finds its right place."}, "wilhelm_lines": {"1": {"text": "Moving too soon, before things are ready, brings humiliation."}, "2": {"text": "Restraint and perseverance, even under pressure, bring good fortune."}, "3": {"text": "Marching ahead before completion brings misfortune; it furthers one to persevere further first."}, "4": {"text": "Perseverance brings good fortune; remorse disappears with sustained effort."}, "5": {"text": "Perseverance brings good fortune, no remorse; sincerity is recognized."}, "6": {"text": "Confidence at the finish is warranted, but overconfidence risks losing what was gained."}}}};
const SYSTEM_PROMPT_BASE =
  "You are Thao, the fortune teller who runs Thao's Fortune Telling Shop, reading the I Ching (Kinh Dich). " +
  "You are the sweetest, most cheerful, most positive person on earth - warm, affectionate, upbeat, generous " +
  "with encouragement, the kind of friend whose energy makes people smile. You have just drawn a hexagram (Que) " +
  "and a specific line (Hao) for a visitor. You'll be given the exact classical Judgment and Hao text - always " +
  "ground your reading in that specific text rather than generic fortune-telling cliches. Speak directly to the " +
  "person, second person, like a real warm conversation, not a written report. No headers, no bullet points. " +
  "\n\n" +
  "BE STRAIGHT TO THE POINT. Your very first sentence must directly answer or address their actual question - " +
  "no throat-clearing, no long windup, no scene-setting before you get there. Your warmth should live in your " +
  "WORD CHOICE and encouragement, not in extra preamble, filler, or decorative buildup that delays the actual " +
  "answer. Cheerful does not mean wordy - be warm AND concise at the same time. " +
  "\n\n" +
  "Being cheerful is about YOUR WARMTH, never about hiding the truth: if a reading is genuinely cautionary or " +
  "mixed, say so honestly, clearly, and immediately - just deliver it the way a loving, positive friend would, " +
  "with care rather than gloom, and always pointing toward what the person can do about it. Keep your first " +
  "reading under 110 words. Keep any follow-up replies under 90 words and stay in character as Thao throughout.";

function buildSystemPrompt(lang){
  if(lang === 'ja'){
    return SYSTEM_PROMPT_BASE + "\n\nIMPORTANT: Respond entirely in natural, warm, conversational Japanese " +
      "(not stiff or overly formal), even though these instructions are in English. The classical Judgment/Hao " +
      "text will be given to you in English - translate or paraphrase it naturally into Japanese as part of your reply.";
  }
  return SYSTEM_PROMPT_BASE;
}

let currentLang = 'en';
const UI = {
  en: {
    subtitle: "a que \u00b7 hao fortune reading",
    badge: "\u2728 live AI reading \u00b7 ask follow-ups",
    qLabel: "Your question",
    qPlaceholder: "e.g. Should I take the new job offer, and why does it feel so hard to decide?",
    queLabel: "Que (1\u201364)", haoLabel: "Hao (1\u20136)",
    castBtn: "Cast the Reading",
    followupPlaceholder: "Ask Thao a follow-up...",
    sendBtn: "Ask",
    resetBtn: "\u2190 start a new reading",
    youName: "You", thaoName: "Thao",
    invalidAlert: "Que must be 1-64 and Hao must be 1-6.",
    noQuestionAlert: "Type a question first - Thao needs something to read for.",
  },
  ja: {
    subtitle: "\u5366\u30fb\u723b\u3067\u898b\u308b\u5360\u3044",
    badge: "\u2728 \u30e9\u30a4\u30d6AI\u30ea\u30fc\u30c7\u30a3\u30f3\u30b0\u30fb\u8ffd\u52a0\u8cea\u554f\u3082OK",
    qLabel: "\u8cea\u554f",
    qPlaceholder: "\u4f8b\uff1a\u65b0\u3057\u3044\u4ed5\u4e8b\u306e\u30aa\u30d5\u30a1\u30fc\u3092\u53d7\u3051\u308b\u3079\u304d\u3067\u3059\u304b\uff1f",
    queLabel: "\u5366\uff081\uff5e64\uff09", haoLabel: "\u723b\uff081\uff5e6\uff09",
    castBtn: "\u5360\u3044\u3092\u884c\u3046",
    followupPlaceholder: "\u30bf\u30aa\u306b\u8ffd\u52a0\u3067\u8cea\u554f...",
    sendBtn: "\u9001\u4fe1",
    resetBtn: "\u2190 \u65b0\u3057\u3044\u5360\u3044\u3092\u59cb\u3081\u308b",
    youName: "\u3042\u306a\u305f", thaoName: "\u30bf\u30aa",
    invalidAlert: "\u5366\u306f1\uff5e64\u3001\u723b\u306f1\uff5e6\u306e\u7bc4\u56f2\u3067\u5165\u529b\u3057\u3066\u304f\u3060\u3055\u3044\u3002",
    noQuestionAlert: "\u307e\u305a\u8cea\u554f\u3092\u5165\u529b\u3057\u3066\u304f\u3060\u3055\u3044\u3002",
  },
};

function applyLanguage(){
  const t = UI[currentLang];
  document.getElementById('subtitleText').textContent = t.subtitle;
  document.getElementById('badgeText').textContent = t.badge;
  document.getElementById('qLabel').textContent = t.qLabel;
  document.getElementById('question').placeholder = t.qPlaceholder;
  document.getElementById('queLabel').textContent = t.queLabel;
  document.getElementById('haoLabel').textContent = t.haoLabel;
  document.getElementById('castBtn').textContent = t.castBtn;
  document.getElementById('followupInput').placeholder = t.followupPlaceholder;
  document.getElementById('sendBtn').textContent = t.sendBtn;
  document.getElementById('resetBtn').textContent = t.resetBtn;
  document.getElementById('langEnBtn').classList.toggle('active', currentLang === 'en');
  document.getElementById('langJaBtn').classList.toggle('active', currentLang === 'ja');
}

function setLang(lang){
  currentLang = lang;
  applyLanguage();
}

let conversation = []; // {role:'user'|'assistant', content:string}
let hexContext = null; // {que, hao, hex, judgment, line, question}

function step(id, delta){
  const el = document.getElementById(id);
  const max = id === 'que' ? 64 : 6;
  let v = parseInt(el.value || '1', 10) + delta;
  if(v < 1) v = 1; if(v > max) v = max;
  el.value = v;
}
function randomize(id, max){ document.getElementById(id).value = Math.floor(Math.random()*max)+1; }

function buildHexlines(que, hao){
  const wrap = document.getElementById('hexlines');
  wrap.innerHTML = '';
  let seed = que * 2654435761 % 2**32;
  for(let i=1; i<=6; i++){
    seed = (seed * 1103515245 + 12345) % 2**31;
    const solid = (seed % 2 === 0);
    const div = document.createElement('div');
    div.className = 'hexline ' + (solid ? 'solid' : 'broken') + (i === hao ? ' active' : '');
    div.innerHTML = solid ? '<div class="bar"></div>' : '<div class="bar bar1"></div><div class="bar bar2"></div>';
    wrap.appendChild(div);
  }
}

function addBubble(role){
  const thread = document.getElementById('chatThread');
  const bubble = document.createElement('div');
  bubble.className = 'bubble ' + (role === 'assistant' ? 'thao' : 'user');
  const name = document.createElement('div');
  name.className = 'bubble-name';
  name.textContent = role === 'assistant' ? UI[currentLang].thaoName : UI[currentLang].youName;
  const inner = document.createElement('div');
  inner.className = 'bubble-inner';
  const nameWrap = document.createElement('div');
  nameWrap.appendChild(name);
  nameWrap.appendChild(inner);
  bubble.appendChild(nameWrap);
  thread.appendChild(bubble);
  thread.scrollTop = thread.scrollHeight;
  return inner;
}

// Streams a Claude response into the given bubble element in real time,
// parsing Server-Sent Events from the Anthropic API's streaming format.
async function streamReply(bubbleEl){
  const cursor = document.createElement('span');
  cursor.className = 'cursor';
  bubbleEl.appendChild(cursor);

  const response = await fetch(WORKER_URL, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      model: "claude-sonnet-4-6",
      max_tokens: 600,
      system: buildSystemPrompt(currentLang),
      messages: conversation,
      stream: true
    })
  });

  if(!response.ok || !response.body){
    throw new Error('API request failed (' + response.status + ')');
  }

  const reader = response.body.getReader();
  const decoder = new TextDecoder();
  let buffer = '';
  let fullText = '';

  while(true){
    const {value, done} = await reader.read();
    if(done) break;
    buffer += decoder.decode(value, {stream:true});
    const lines = buffer.split('\n');
    buffer = lines.pop();
    for(const rawLine of lines){
      const line = rawLine.trim();
      if(!line.startsWith('data:')) continue;
      const jsonStr = line.slice(5).trim();
      if(!jsonStr || jsonStr === '[DONE]') continue;
      let evt;
      try{ evt = JSON.parse(jsonStr); } catch(e){ continue; }
      if(evt.type === 'content_block_delta' && evt.delta && evt.delta.text){
        fullText += evt.delta.text;
        bubbleEl.textContent = fullText;
        bubbleEl.appendChild(cursor);
        bubbleEl.parentElement.parentElement.scrollTop = bubbleEl.parentElement.parentElement.scrollHeight;
      }
    }
  }
  cursor.remove();
  return fullText;
}

async function startReading(){
  const que = parseInt(document.getElementById('que').value, 10);
  const hao = parseInt(document.getElementById('hao').value, 10);
  const question = document.getElementById('question').value.trim();

  if(!que || que < 1 || que > 64 || !hao || hao < 1 || hao > 6){
    alert(UI[currentLang].invalidAlert); return;
  }
  if(!question){
    alert(UI[currentLang].noQuestionAlert); return;
  }

  const hex = OFFLINE_DATA[String(que)];
  const judgment = hex.wilhelm_judgment.text;
  const line = hex.wilhelm_lines[String(hao)].text;
  hexContext = {que, hao, hex, judgment, line, question};

  document.getElementById('setupForm').style.display = 'none';
  document.getElementById('hexlineWrap').style.display = 'flex';
  document.getElementById('queTitle').style.display = 'block';
  document.getElementById('queSub').style.display = 'block';
  document.getElementById('classicalBox').style.display = 'block';
  document.getElementById('chatThread').style.display = 'block';
  document.getElementById('followupRow').style.display = 'flex';
  document.getElementById('resetLink').style.display = 'block';

  buildHexlines(que, hao);
  document.getElementById('queTitle').textContent = 'Que ' + que + ': ' + hex.english;
  document.getElementById('queSub').textContent = 'Hao ' + hao;
  document.getElementById('classicalBox').textContent =
    'Judgment: ' + judgment + '  \u2014  Hao ' + hao + ': ' + line;

  const openingPrompt =
    'I drew Que ' + que + ' (' + hex.english + '), Hao ' + hao + '.\n' +
    'Judgment: "' + judgment + '"\n' +
    'Hao ' + hao + ': "' + line + '"\n' +
    'My question: "' + question + '"';

  conversation.push({role:'user', content: openingPrompt});
  addBubble('user').textContent = question; // show just their question in the chat, not the raw data dump
  const thaoBubble = addBubble('assistant');
  await runStream(thaoBubble);
}

async function sendFollowup(){
  const input = document.getElementById('followupInput');
  const text = input.value.trim();
  if(!text) return;
  input.value = '';
  document.getElementById('sendBtn').disabled = true;
  input.disabled = true;

  conversation.push({role:'user', content: text});
  addBubble('user').textContent = text;
  const thaoBubble = addBubble('assistant');
  await runStream(thaoBubble);

  document.getElementById('sendBtn').disabled = false;
  input.disabled = false;
  input.focus();
}

async function runStream(bubbleEl){
  document.getElementById('errorBox').innerHTML = '';
  document.getElementById('castBtn').disabled = true;
  document.getElementById('sendBtn').disabled = true;
  try{
    const fullText = await streamReply(bubbleEl);
    conversation.push({role:'assistant', content: fullText});
  } catch(err){
    document.getElementById('errorBox').innerHTML =
      '<div class="error-box">Something went wrong reaching Thao: ' + err.message + '</div>';
    bubbleEl.parentElement.parentElement.remove ? null : null;
  } finally {
    document.getElementById('castBtn').disabled = false;
    document.getElementById('sendBtn').disabled = false;
  }
}

function resetAll(){
  conversation = [];
  hexContext = null;
  document.getElementById('chatThread').innerHTML = '';
  document.getElementById('question').value = '';
  document.getElementById('setupForm').style.display = 'block';
  document.getElementById('hexlineWrap').style.display = 'none';
  document.getElementById('queTitle').style.display = 'none';
  document.getElementById('queSub').style.display = 'none';
  document.getElementById('classicalBox').style.display = 'none';
  document.getElementById('chatThread').style.display = 'none';
  document.getElementById('followupRow').style.display = 'none';
  document.getElementById('resetLink').style.display = 'none';
  document.getElementById('errorBox').innerHTML = '';
}

applyLanguage();
</script>
</body>
</html>
