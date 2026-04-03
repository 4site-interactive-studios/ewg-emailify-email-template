# CLAUDE.md

## Comparison Screenshots

When both `index.mjml` and `index-simple.mjml` exist in the repo, regenerate `before-and-after.jpg` in these situations:
- When explicitly requested
- Before merging to `main` or any target branch

The comparison image shows side-by-side renders of both files across 2 modes:
1. Desktop (700px viewport)
2. Mobile (400px viewport)

### How to regenerate

1. Compile both MJML files to HTML: `npx mjml index.mjml -o /tmp/original.html && npx mjml index-simple.mjml -o /tmp/simple.html`
2. Serve them locally: `node -e "require('http').createServer((req,res)=>{res.writeHead(200,{'Content-Type':'text/html'});res.end(require('fs').readFileSync('/tmp'+(req.url==='/'?'/original':req.url)+'.html'))}).listen(8765)"`
3. Run the capture script: `node /tmp/capture-comparisons.js .` (outputs `before-and-after.jpg` to the specified directory)
4. The script uses Puppeteer to screenshot both pages at each viewport size, composites them side-by-side with labels, and stacks both into a single JPEG.
