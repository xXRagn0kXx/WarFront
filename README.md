crear con The Rust + WebAssembly + WebGPU un juego multijugador via web que  sera tipo Battlefield y se jugara a traves de navegador (shooter con vehiculos, bases, escenarios destruibles, conquista de bandera, lucha por equipos en 2 facciones, EEUU y RUSIA, , 4 profesiones con diferentes armas a escoger) con graficos low poly a lo minecraft o roblox, sera de 5 contra 5,   A la hora de realizar el codigo debe priorizarse la eficiencia del codigo y la funcionalidad del juego, sera ofrecido a traves de nginx y la gente accedera a traves del navegador web poniendo la URL , la idea es que la gente entre, escoja un nick , una profesion de soldado (asalto, francotirador, ingeniero , medico) y una faccion, (Rusia o EEUU) al hacerlo se respauneara en una base aliada donde habra vehiculos,  
equipo asalto 
pistola 
m16
2 granadas

Equipo ingeniero
uzi
bazooka
2 minas antitanque

francotirador
Pistola
Sniper
Municion

Medico
Uzi
Rayo laser que cura
2 pack de 50 escudo


Cazas y tankes tendran amtrelladora + bazooka ,

Prerequisites
sudo apt update
sudo apt install build-essential curl nginx git
# Install Rust if not present
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
# Install wasm-pack
curl https://rustwasm.github.io/wasm-pack/installer/init.sh -sSf | sh
Compilation
Build Client (Wasm):

cd client
wasm-pack build --target web --release
This generates pkg/ inside client/.

Build Server:

cd server
cargo build --release
Binary will be at target/release/server.

Installation / Running
Nginx Configuration: Create /etc/nginx/sites-available/frontwar with correct MIME types for .wasm.

server {
    listen 80;
    server_name localhost;
    root /srv/FrontWar/www; # We will create this
    
    location / {
        try_files $uri $uri/ /index.html;
    }
    
    types {
        application/wasm wasm;
    }
}
Link it: sudo ln -s /etc/nginx/sites-available/frontwar /etc/nginx/sites-enabled/ Restart nginx: sudo systemctl restart nginx

Run Server:

./target/release/server
Access: Open browser at http://localhost.
