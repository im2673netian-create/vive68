from pathlib import Path
import zipfile, textwrap

root = Path("/mnt/data/precto_hair_shop")
root.mkdir(exist_ok=True)

html = r'''<!doctype html>
<html lang="ko">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>PRECTO HAIR | 프렉토 헤어</title>
<style>
*{box-sizing:border-box}body{margin:0;font-family:Arial,"Noto Sans KR",sans-serif;background:#f7f5f2;color:#222}
header{position:sticky;top:0;background:#fff;z-index:5;border-bottom:1px solid #eee}
.top{max-width:1180px;margin:auto;height:72px;display:flex;align-items:center;gap:28px;padding:0 20px}
.logo{font-size:25px;font-weight:800;letter-spacing:3px}.logo small{display:block;font-size:9px;letter-spacing:4px;color:#8c8175;text-align:center}
nav{display:flex;gap:22px;font-size:14px;flex:1}nav span{cursor:pointer}.icons{display:flex;gap:14px;font-size:20px}
.hero{max-width:1180px;margin:20px auto;padding:0 20px}.banner{height:360px;border-radius:18px;overflow:hidden;background:linear-gradient(120deg,#dfd3c5,#f4eee8 55%,#c9b7a7);display:flex;align-items:center;padding:55px}
.banner h1{font-size:45px;letter-spacing:1px;margin:0 0 14px}.banner p{line-height:1.7;color:#5c5148}.btn{border:0;background:#222;color:white;padding:14px 25px;border-radius:5px;cursor:pointer}
.wrap{max-width:1180px;margin:42px auto;padding:0 20px}.title{display:flex;justify-content:space-between;align-items:end;margin-bottom:20px}.title h2{margin:0;font-size:24px}.title small{color:#888}
.cats{display:flex;gap:10px;overflow:auto;margin-bottom:25px}.cat{background:#fff;border:1px solid #ddd;border-radius:30px;padding:11px 18px;white-space:nowrap;cursor:pointer}.cat.active{background:#222;color:#fff}
.grid{display:grid;grid-template-columns:repeat(4,1fr);gap:18px}.card{background:#fff;border-radius:12px;overflow:hidden;box-shadow:0 2px 10px #0000000a}.pic{height:220px;display:flex;align-items:center;justify-content:center;background:#eee8e1}.bottle{width:82px;height:145px;border-radius:12px 12px 18px 18px;background:linear-gradient(90deg,#222,#555,#191919);position:relative;box-shadow:0 12px 25px #0003}.bottle:before{content:"PRECTO";position:absolute;top:55px;left:10px;color:#e7d8c5;font-size:12px;letter-spacing:1px}.bottle:after{content:"HAIR";position:absolute;top:76px;left:22px;color:#fff;font-size:8px;letter-spacing:2px}.cap{position:absolute;width:58px;height:20px;top:-17px;left:12px;background:#171717;border-radius:4px}
.info{padding:15px}.tag{font-size:11px;color:#9a6f48}.name{font-weight:700;margin:6px 0}.price{font-size:17px;font-weight:800}.old{text-decoration:line-through;color:#aaa;font-size:12px;margin-left:5px}.add{margin-top:12px;width:100%;padding:10px;border:1px solid #222;background:#fff;border-radius:5px;cursor:pointer}
.notice{background:#222;color:#fff;padding:15px;border-radius:10px;margin-top:35px;text-align:center}
footer{margin-top:60px;background:#222;color:#ccc;padding:40px 20px}.foot{max-width:1180px;margin:auto}.cart{position:fixed;right:24px;bottom:24px;background:#222;color:#fff;border-radius:50%;width:58px;height:58px;display:grid;place-items:center;font-size:21px;box-shadow:0 8px 25px #0004;cursor:pointer}.badge{position:absolute;right:-2px;top:-3px;background:#c88b5c;color:white;border-radius:50%;font-size:10px;width:19px;height:19px;text-align:center;padding-top:3px}
@media(max-width:800px){nav{display:none}.grid{grid-template-columns:repeat(2,1fr)}.banner{height:300px;padding:30px}.banner h1{font-size:31px}.pic{height:180px}}
@media(max-width:450px){.grid{grid-template-columns:1fr 1fr}.top{gap:12px}.logo{font-size:20px}.banner{border-radius:12px}}
</style>
</head>
<body>
<header><div class="top">
<div class="logo">PRECTO<small>HAIR COSMETICS</small></div>
<nav><span onclick="filter('전체')">전체상품</span><span onclick="filter('샴푸')">샴푸</span><span onclick="filter('트리트먼트')">트리트먼트</span><span onclick="filter('에센스')">에센스</span><span onclick="filter('전문가용')">전문가용</span></nav>
<div class="icons">♡　⌕　♙</div></div></header>

<section class="hero"><div class="banner"><div><div class="tag">PRECTO PROFESSIONAL</div><h1>당신의 머릿결에<br>프렉토를 더하다.</h1><p>살롱의 전문 케어를 집에서도.<br>프렉토 헤어케어 베스트 제품을 만나보세요.</p><button class="btn" onclick="document.getElementById('products').scrollIntoView()">SHOP NOW</button></div></div></section>

<section class="wrap" id="products">
<div class="title"><h2>BEST SELLER</h2><small>프렉토 인기상품</small></div>
<div class="cats"><button class="cat active" onclick="filter('전체')">전체</button><button class="cat" onclick="filter('샴푸')">샴푸</button><button class="cat" onclick="filter('트리트먼트')">트리트먼트</button><button class="cat" onclick="filter('에센스')">에센스</button><button class="cat" onclick="filter('전문가용')">전문가용</button></div>
<div class="grid" id="grid"></div>
<div class="notice">네이버 스마트스토어 · 쿠팡 상품 연동 영역<br><small>※ 실제 판매 연동은 각 서비스의 공식 API/제휴 정책에 맞춰 연결합니다.</small></div>
</section>

<footer><div class="foot"><b>PRECTO HAIR</b><p>헤어살롱 전문가를 위한 프리미엄 헤어케어 쇼핑몰</p><small>고객센터 0000-0000 · 사업자정보 · 이용약관 · 개인정보처리방침</small></div></footer>
<div class="cart" onclick="alert('장바구니 상품이 '+cart+'개 있습니다.')">🛒<span class="badge" id="badge">0</span></div>

<script>
const products=[
["샴푸","프렉토 리페어 샴푸 500ml","24,900원","29,900원"],
["트리트먼트","프렉토 딥 리페어 트리트먼트 500ml","28,900원","35,000원"],
["에센스","프렉토 아르간 헤어 에센스 100ml","19,800원","24,000원"],
["전문가용","프렉토 살롱 케어 마스크 1000ml","39,900원","49,000원"],
["샴푸","프렉토 볼륨 샴푸 500ml","25,900원","31,000원"],
["트리트먼트","프렉토 모이스처 트리트먼트","27,900원","34,000원"],
["에센스","프렉토 퍼퓸 헤어 오일","21,900원","27,000원"],
["전문가용","프렉토 프로 리본딩 케어","45,900원","55,000원"]
];
let cart=0;
function render(type='전체'){
 const list=type==='전체'?products:products.filter(x=>x[0]===type);
 document.getElementById('grid').innerHTML=list.map((p,i)=>`
 <article class="card"><div class="pic"><div class="bottle"><div class="cap"></div></div></div>
 <div class="info"><div class="tag">${p[0]}</div><div class="name">${p[1]}</div><div class="price">${p[2]} <span class="old">${p[3]}</span></div>
 <button class="add" onclick="addCart()">장바구니 담기</button></div></article>`).join('');
}
function addCart(){cart++;document.getElementById('badge').textContent=cart}
function filter(type){document.querySelectorAll('.cat').forEach(x=>x.classList.toggle('active',x.textContent===type));render(type)}
render();
</script>
</body></html>'''

(root/"index.html").write_text(html, encoding="utf-8")
readme = """# PRECTO HAIR 쇼핑몰 프로토타입
index.html을 크롬에서 열면 바로 확인할 수 있습니다.

포함:
- 프렉토 헤어 브랜드 메인 화면
- 카테고리 필터
- 상품 카드/가격/할인 표시
- 장바구니 카운트
- 모바일 반응형
- 네이버/쿠팡 공식 연동 영역

실서비스 단계에서는 네이버 스마트스토어/API 및 쿠팡 파트너스 등 허용된 공식 연동 방식과 실제 상품 이미지/사업자 정보를 연결하면 됩니다.
"""
(root/"README.txt").write_text(readme, encoding="utf-8")
zip_path="/mnt/data/프렉토_헤어_쇼핑몰_프로토타입.zip"
with zipfile.ZipFile(zip_path,"w",zipfile.ZIP_DEFLATED) as z:
    for f in root.iterdir(): z.write(f, f.name)
print(zip_path)
