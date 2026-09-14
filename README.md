# Hướng dẫn tùy chỉnh 3DVista Tour (W03)

## Ẩn mục version "3DVista Player v:0.2365" trong menu

Mục cuối cùng của menu mặc định (hamburger) hiển thị branding + version của player.
Cách ẩn:

### Bước 1: Tìm đoạn code

File: `lib/tdvplayer.js` — module `tdv/player/view/Menu`, hàm `Q.prototype.Eya` (hàm build danh sách menu).

Tìm đoạn:

```js
Y=[].concat(e.name?[e.name]:[],e.seb!="false"?e.ZX?["v:"+e.ZX+"."+e.eY]:["v:"+e.eY]:[]).join(" ");Y!=""&&(T.length>0&&T.push(""),T.push(Y),this.SL.push(this.m6a.bind(this)));return T};
```

Đoạn này nối `e.name` ("3DVista Player") + `"v:"+MAJOR+"."+MINOR` thành 1 mục menu,
và gán click handler `m6a` (mở link 3DVista).

### Bước 2: Sửa thành

```js
return T};
```

### Bước 3: Kiểm tra

```bash
node --check lib/tdvplayer.js
```

Ra `SYNTAX_OK` là ổn. Mở tour xem menu không còn mục version.

## Các chỗ liên quan đến version trong `lib/tdvplayer.js`

| Vị trí | Nội dung | Ghi chú |
|---|---|---|
| `h.prototype.getVersion=h.prototype.fMa=function(){return{MAJOR:"0",MINOR:"2365"}}` | Hàm lấy version | Chỉ dùng ghép param `swv=0.2365` cho service worker, KHÔNG hiển thị UI |
| `define("tdv/player/AppInfo"...)` — `h.ZX=parseInt("0");h.eY=parseInt("2365")` | Giá trị MAJOR/MINOR | Nguồn số version, tên hiển thị `h.name` là chuỗi mã hóa (giải ra "3DVista Player") |
| Module `tdv/player/view/Menu`, hàm `Eya` | Mục menu hiển thị `"3DVista Player v:0.2365"` | ĐÃ ẨN (xem hướng dẫn trên) |
| Module `tdv/player/Main` — `r.log(w.join(" "))` | Log version ra console | Không phải UI, để nguyên |

## Lưu ý

- Sau mỗi lần export lại từ 3DVista, `lib/tdvplayer.js` bị ghi đè → phải áp lại thao tác trên.
- Số dòng có thể thay đổi giữa các bản export, nên tìm theo nội dung code (`Y=[].concat(e.name?...`), không tìm theo số dòng.
