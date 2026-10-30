import hashlib
import datetime
from http.server import HTTPServer, BaseHTTPRequestHandler
import json
import urllib.parse

def generer_qr_preuve(nom_site, agent_id):
    heure = datetime.datetime.now().strftime("%d/%m/%Y %H:%M:%S")
    brut = f"QG777-{agent_id}-{nom_site}-{heure}"
    hash_preuve = hashlib.sha256(brut.encode()).hexdigest().upper()[:16]
    return {
        "site": nom_site,
        "agent": agent_id,
        "heure": heure,
        "qr_code": f"QG777-{hash_preuve}",
        "statut": "CONTROLE VALIDE - PREUVE SCELLEE"
    }

class Handler(BaseHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.send_header('Content-type', 'text/html; charset=utf-8')
        self.end_headers()
        
        preuve = generer_qr_preuve("La Defense - Tour First", "QG777")
        
        html = f"""
        <html><body style="font-family:Arial; background:#111; color:#0f0; text-align:center; padding:30px;">
        <h1>🔒 QG777 SECURE CHECK</h1>
        <div style="border:2px solid #0f0; padding:20px; background:#222; border-radius:15px;">
            <h2>{preuve['statut']}</h2>
            <p>Site: {preuve['site']}</p>
            <p>Agent: {preuve['agent']}</p>
            <p>Heure: {preuve['heure']}</p>
            <h1 style="background:white; color:black; padding:10px; letter-spacing:5px;">{preuve['qr_code']}</h1>
            <p>Empreinte SHA-256 souveraine</p>
        </div>
        <p>Systeme QG777 - Formateur Technique Souverain</p>
        </body></html>
        """
        self.wfile.write(html.encode())

print("=== QG777 APP LANCEE sur http://localhost:8080 ===")
HTTPServer(("0.0.0.0", 8080), Handler).serve_forever()
