<!DOCTYPE html>
<html lang="ta">
<head>
<meta charset="UTF-8">
<title>Tamil Worship PPT – Uni Ila.Sundaram-06 Font</title>

<script src="https://unpkg.com/pptxgenjs/dist/pptxgen.bundle.js"></script>

<style>
/* Embed Uni Ila.Sundaram-06 font */
@font-face {
    font-family: 'UniIlaSundaram06';
    src: url('fonts/UniIlaSundaram-06.ttf') format('truetype');
    font-weight: normal;
    font-style: normal;
}

html,body{
    height:100%;margin:0;
    background:#27032e;color:#fff;
    font-family:'UniIlaSundaram06', serif;
}
body{display:flex}

/* LEFT PANEL */
.controls{
    width:65%;padding:20px;box-sizing:border-box;
}
input,textarea,select{
    width:100%;font-size:16px;padding:8px;
    margin-top:6px;background:#393E46;color:#fff;
    border:none;border-radius:6px;
}
textarea{height:220px}
label{margin-top:10px;display:block;font-weight:bold}

h1{font-size:52px;margin:0}
h3{margin:4px 0 10px}

/* options row */
.option-row{
    display:flex;gap:14px;align-items:center;margin-top:6px
}
.option-row input[type=color]{width:34px;height:26px;border:none}
.option-row input[type=checkbox]{width:auto;height:auto}

/* Buttons */
button{
    margin-top:14px;padding:12px 22px;
    font-size:16px;font-weight:bold;
    color:#fff;border:none;border-radius:10px;
    background:linear-gradient(45deg,#ff4d8d,#ff7f50);
    cursor:pointer;
}

/* RIGHT PANEL */
.preview-panel{
    width:35%;background:#140b24;
    padding:10px;overflow-y:auto;
}
.preview-title{text-align:center;font-weight:bold;margin-bottom:10px}

.slide-thumb{
    width:100%;aspect-ratio:16/9;
    margin-bottom:12px;position:relative;
    border-radius:10px;overflow:hidden;
    background:#000;
}
.thumb-bg{
    position:absolute;inset:0;
    background-size:cover;background-position:center;
}
.thumb-overlay{
    position:absolute;inset:0;
    background:rgba(0,0,0,.55);
}
.thumb-text{
    position:absolute;inset:0;
    display:flex;align-items:center;justify-content:center;
    text-align:center;padding:16px;
    white-space:pre-line;font-weight:900; /* bold 900 */
}
</style>
</head>

<body>

<div class="controls">
<h1>Beth-El Media</h1>
<h3>🎤 Tamil Worship PPT Generator</h3>

<label>PPT Name</label>
<input id="pptName" value="Tamil_Worship_PPT">

<label>Background Image</label>
<input type="file" id="bgImage" accept="image/*">

<label>Text Options</label>
<div class="option-row">
    <input type="color" id="textColor" value="#ffffff"> Text Color
</div>

<label>Outline Options</label>
<div class="option-row">
    <input type="checkbox" id="outlineToggle" checked> Enable Outer Outline
    <input type="color" id="outlineColor" value="#000000"> Outline Color
</div>

<label>Unicode Tamil Lyrics</label>
<textarea id="textInput" placeholder="Blank line = New Slide"></textarea>

<button onclick="renderPreview()">Preview</button>
<button onclick="generatePPT()">Download PPT</button>
</div>

<div class="preview-panel">
<div class="preview-title">📽 Slide Preview</div>
<div id="previewList"></div>
</div>

<script>
let bgData = null;
const outlineSizeDefault = 3; // outer-only

bgImage.onchange = e=>{
    const f = e.target.files[0];
    if(!f) return;
    const r = new FileReader();
    r.onload = ()=>{bgData=r.result; renderPreview();}
    r.readAsDataURL(f);
};

function calcFontSize(text){
    const lines = text.split("\n").length;
    let size = 42;
    if(lines > 4) size -= (lines-4)*4;
    return Math.max(size,24);
}

function renderPreview(){
    const txt = textInput.value.trim();
    if(!txt){ alert("Lyrics enter pannunga da 🙂"); return; }
    previewList.innerHTML = "";

    const outlineOn = outlineToggle.checked;
    const o = outlineSizeDefault;

    txt.split(/\n\s*\n/).forEach(slide=>{
        const s = document.createElement("div");
        s.className = "slide-thumb";

        if(bgData){
            const bg = document.createElement("div");
            bg.className = "thumb-bg";
            bg.style.backgroundImage = `url(${bgData})`;
            s.append(bg);
            s.append(Object.assign(document.createElement("div"), {className:"thumb-overlay"}));
        }

        const t = document.createElement("div");
        t.className = "thumb-text";
        const fs = calcFontSize(slide);
        t.style.fontSize = fs + "px";
        t.style.lineHeight = (fs+8) + "px";
        t.style.fontFamily = 'UniIlaSundaram06';
        t.style.fontWeight = '900'; // bold 900
        t.style.color = textColor.value;

        if(outlineOn){
            // Outer-only outline effect
            t.style.textShadow = `
             -${o}px -${o}px 0 ${outlineColor.value},
              ${o}px -${o}px 0 ${outlineColor.value},
             -${o}px  ${o}px 0 ${outlineColor.value},
              ${o}px  ${o}px 0 ${outlineColor.value},
              0px -${o}px 0 ${outlineColor.value},
             -${o}px 0px 0 ${outlineColor.value},
              ${o}px 0px 0 ${outlineColor.value},
              0px ${o}px 0 ${outlineColor.value}`;
        } else {
            t.style.textShadow = 'none';
        }

        t.innerText = slide;
        s.append(t);
        previewList.append(s);
    });
}

function generatePPT(){
    const pptx = new PptxGenJS();
    const outlineOn = outlineToggle.checked;

    textInput.value.trim().split(/\n\s*\n/).forEach(slide=>{
        const s = pptx.addSlide();
        if(bgData) s.background = {data:bgData};
        else s.background = {fill:"140b24"};

        const fs = calcFontSize(slide)+8;
        s.addText(slide,{
            x:0.3, y:0, w:9.4, h:"100%",
            align:"center", valign:"middle",
            fontFace:'UniIlaSundaram06',
            fontSize:fs,
            bold:true,
            color:textColor.value.replace("#",""),
            outline: outlineOn ? {size:outlineSizeDefault,color:outlineColor.value.replace("#","")} : undefined
        });
    });
    pptx.writeFile(pptName.value+".pptx");
}
</script>

</body>
</html>
