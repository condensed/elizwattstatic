# Troubleshooting: Assets Not Loading on /thoughts/page/2/

## Current Status

The paths in `/thoughts/page/2/index.html` are **CORRECT**:
- CSS: `href="../../../wp-content/themes/watt-scb/style.css"` ✅
- JS: `src="../../../wp-content/themes/watt-scb/js/modernizr.custom.js"` ✅
- Navigation links: `href="../../../about/"` ✅

## Verification Tests

### Test 1: HTTP Server Running
```bash
curl -I http://localhost:8000/thoughts/page/2/
# Should return: HTTP/1.0 200 OK
```

### Test 2: CSS File Accessible
```bash
curl -I http://localhost:8000/wp-content/themes/watt-scb/style.css
# Should return: HTTP/1.0 200 OK
```

### Test 3: Path Resolution
From `/thoughts/page/2/index.html`:
- `../../../` goes up 3 levels: `/thoughts/page/2/` → `/thoughts/page/` → `/thoughts/` → `/`
- Then: `/` + `wp-content/themes/watt-scb/style.css`
- Final: `/wp-content/themes/watt-scb/style.css` ✅

## Common Issues & Solutions

### Issue 1: Opening File Directly (file:// protocol)
**Symptom:** Assets don't load, paths don't work
**Cause:** Opening `index.html` by double-clicking or using "Open File" in browser
**Browser shows:** `file:///Users/shaunsantacruz/Downloads/.../index.html`
**Solution:** Must use HTTP server

```bash
cd /Users/shaunsantacruz/Downloads/simply-static-1-1761684244
python3 -m http.server 8000
# Then visit: http://localhost:8000/thoughts/page/2/
```

### Issue 2: Missing Trailing Slash
**Symptom:** Relative paths resolve incorrectly
**Cause:** Visiting `http://localhost:8000/thoughts/page/2` (no slash)
**Solution:** Always use trailing slash: `http://localhost:8000/thoughts/page/2/`

### Issue 3: Browser Cache
**Symptom:** Old content showing
**Solution:** Hard refresh (Cmd+Shift+R on Mac, Ctrl+Shift+R on Windows)

### Issue 4: Wrong Base Path
**Symptom:** All relative links broken
**Check:** View page source, look for `<base>` tag
**Solution:** Should have no `<base>` tag (currently correct)

## Debugging Steps

1. **Check URL in browser address bar**
   - Must start with `http://localhost:8000/`
   - NOT `file:///`

2. **Open browser Developer Tools** (F12 or Cmd+Option+I)
   - Go to "Network" tab
   - Reload page
   - Look for failed requests (red items)
   - Check what URLs are being requested

3. **Check Console tab**
   - Look for errors like:
     - "Failed to load resource"
     - "net::ERR_FILE_NOT_FOUND"
     - "Cross-Origin Request Blocked"

4. **Test CSS directly**
   - Visit: `http://localhost:8000/wp-content/themes/watt-scb/style.css`
   - Should show CSS code, not 404

5. **Test from different pages**
   - `http://localhost:8000/` - should load with styles ✅
   - `http://localhost:8000/about/` - should load with styles ✅
   - `http://localhost:8000/page/2/` - should load with styles ✅
   - `http://localhost:8000/thoughts/page/2/` - should load with styles ❓

## What To Check Next

Please provide:
1. **Exact URL in browser:** (copy from address bar)
2. **Browser Console errors:** (F12 → Console tab → screenshot or copy errors)
3. **Network tab failed requests:** (F12 → Network tab → any red/failed items)
4. **What you see:** 
   - Unstyled HTML text only?
   - Blank page?
   - 404 error?
   - Something else?

## File Path Verification

Test these commands in terminal:

```bash
# Verify files exist
ls -la /Users/shaunsantacruz/Downloads/simply-static-1-1761684244/wp-content/themes/watt-scb/style.css
ls -la /Users/shaunsantacruz/Downloads/simply-static-1-1761684244/thoughts/page/2/index.html

# Test path resolution
cd /Users/shaunsantacruz/Downloads/simply-static-1-1761684244/thoughts/page/2
realpath ../../../wp-content/themes/watt-scb/style.css

# Test HTTP server responses
curl -I http://localhost:8000/thoughts/page/2/
curl -I http://localhost:8000/wp-content/themes/watt-scb/style.css
```

## Expected Results

If everything is working correctly:
- ✅ Page loads at `http://localhost:8000/thoughts/page/2/`
- ✅ Page has styling (background, fonts, layout)
- ✅ Navigation links work
- ✅ Browser console has no errors
- ✅ Network tab shows all CSS/JS files loaded (status 200)

## Current Situation

**Paths are fixed:** ✅ All paths use correct `../../../` format
**Files exist:** ✅ CSS, JS, and HTML files are present
**HTTP server:** ✅ Server responds with 200 status codes
**Missing:** ❓ Need to know exact error you're seeing

---

**Next Step:** Please share the exact error or what you're seeing in the browser so we can pinpoint the exact issue.

