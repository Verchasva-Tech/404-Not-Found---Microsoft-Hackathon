# 404-Not-Found---Microsoft-Hackathon
This repository is regarding the Microsoft Hackathon displaying the working website for our problem statement. 

import http.server
import socketserver
import threading
import webbrowser
from pathlib import Path

PORT = 8000
FILE = "spotting-at-risk-early-9.html"

# Serve files from the folder this script lives in
directory = Path(__file__).parent.resolve()

if not (directory / FILE).exists():
    raise SystemExit(f"Put {FILE} in the same folder as this script: {directory}")


class Handler(http.server.SimpleHTTPRequestHandler):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, directory=str(directory), **kwargs)


socketserver.TCPServer.allow_reuse_address = True

with socketserver.TCPServer(("127.0.0.1", PORT), Handler) as server:
    url = f"http://localhost:{PORT}/{FILE}"
    print(f"Serving at {url}  (Ctrl+C to stop)")
    threading.Timer(0.5, lambda: webbrowser.open(url)).start()
    try:
        server.serve_forever()
    except KeyboardInterrupt:
        print("\nServer stopped.")
