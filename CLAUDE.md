# CLAUDE.md

## Comparison Screenshots

When both `index.mjml` and `index-simple.mjml` exist in the repo, regenerate `before-and-after.jpg` in these situations:
- When explicitly requested
- Before merging to `main` or any target branch

The comparison is split into two images:
- `before-and-after-desktop.jpg` — Desktop (700px viewport)
- `before-and-after-mobile.jpg` — Mobile (400px viewport)

Each image shows the original on the left and simplified on the right.

### How to regenerate

1. Compile both MJML files to HTML: `npx mjml index.mjml -o /tmp/original.html && npx mjml index-simple.mjml -o /tmp/simple.html`
2. Serve them locally: `node -e "require('http').createServer((req,res)=>{res.writeHead(200,{'Content-Type':'text/html'});res.end(require('fs').readFileSync('/tmp'+(req.url==='/'?'/original':req.url)+'.html'))}).listen(8765)"`
3. Run the capture script: `node /tmp/capture-comparisons.js .` (outputs comparison PNGs to the specified directory)
4. Convert to JPEG: `sips -s format jpeg compare-desktop.png --out before-and-after-desktop.jpg -s formatOptions 85` (repeat for mobile)
5. The script uses Puppeteer to screenshot both pages at each viewport size and composites them side-by-side with labels.
