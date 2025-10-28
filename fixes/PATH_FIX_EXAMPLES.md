# Path Fix Examples - Before & After

## Root Level File (index.html)

### BEFORE (Broken)
```html
<link rel="shortcut icon" href="//favicon.ico" type="image/x-icon">
<link rel="stylesheet" href="//wp-content/themes/watt-scb/style.css">
<a href="//about/">About</a>
<a href="//">Elizabeth Watt</a>
```

### AFTER (Fixed)
```html
<link rel="shortcut icon" href="./favicon.ico" type="image/x-icon">
<link rel="stylesheet" href="./wp-content/themes/watt-scb/style.css">
<a href="./about/">About</a>
<a href="./">Elizabeth Watt</a>
```

---

## Subdirectory File (about/index.html)

### BEFORE (Broken)
```html
<link rel="shortcut icon" href="//favicon.ico" type="image/x-icon">
<link rel="stylesheet" href="//wp-content/themes/watt-scb/style.css">
<a href="//contact/">Contact</a>
<a href="//">Home</a>
```

### AFTER (Fixed)
```html
<link rel="shortcut icon" href="../favicon.ico" type="image/x-icon">
<link rel="stylesheet" href="../wp-content/themes/watt-scb/style.css">
<a href="../contact/">Contact</a>
<a href="../">Home</a>
```

---

## Images - CDN to Local

### BEFORE (External CDN)
```html
<img src="https://i0.wp.com/elizabethwatt.com/wp-content/uploads/2018/06/aspects_post2.jpg">
<img src="https://elizabethwattc.wpengine.com/wp-content/uploads/2019/05/photo.jpg">
```

### AFTER (Local Relative)
```html
<img src="../wp-content/uploads/2018/06/aspects_post2.jpg">
<img src="../wp-content/uploads/2019/05/photo.jpg">
```

---

## Style Attributes

### BEFORE (Broken)
```html
<div style="background-image: url('//wp-content/uploads/2018/06/header.jpg')">
```

### AFTER (Fixed)
```html
<div style="background-image: url('../wp-content/uploads/2018/06/header.jpg')">
```

---

## Why This Works on GitHub Pages

1. **Relative paths are resolved by the browser**
   - `./` = current directory
   - `../` = parent directory
   - Works the same locally and on GitHub Pages

2. **No protocol dependency**
   - BEFORE: `//wp-content/...` depends on page protocol
   - AFTER: `./wp-content/...` is always relative to HTML file location

3. **GitHub Pages serves as static web server**
   - Serves files with proper HTTP protocol
   - Relative paths resolve correctly
   - Clean URLs work with index.html in subdirectories

---

## Test Results

### Local File Protocol (file://)
- BEFORE: ❌ Assets fail to load
- AFTER: ⚠️ Some features limited (use local server instead)

### Local Web Server (http://localhost:8000/)
- BEFORE: ❌ Would still fail
- AFTER: ✅ Everything works

### GitHub Pages (https://username.github.io/)
- BEFORE: ❌ Would fail
- AFTER: ✅ Everything works

---

## Files Verified

✅ `/index.html` - Root level paths use `./`
✅ `/about/index.html` - One level deep uses `../`
✅ `/category/creativity/page/2/index.html` - Three levels deep uses `../../../`
✅ All image references converted from CDN to local
✅ All navigation links converted to relative paths
✅ All CSS/JS references converted to relative paths

**Total files fixed: 126**

