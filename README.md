<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tabla Periódica Interactiva Pro - 118 Elementos</title>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128/examples/js/controls/OrbitControls.js"></script>

    <style>
        body { margin: 0; background: #0b1220; color: white; font-family: 'Segoe UI', sans-serif; overflow-x: hidden; }
        header { padding: 20px; text-align: center; background: #1e293b; position: sticky; top: 0; z-index: 100; box-shadow: 0 4px 15px rgba(0,0,0,0.5); }
        input { padding: 12px 20px; border-radius: 25px; border: none; width: 60%; background: #334155; color: white; outline: none; border: 1px solid #475569; transition: 0.3s; }
        input:focus { border-color: #3b82f6; width: 65%; }
        
        #btn-biblioteca { position: fixed; top: 15px; right: 20px; font-size: 28px; background: #3b82f6; width: 55px; height: 55px; 
            display: flex; justify-content: center; align-items: center; border-radius: 50%; cursor: pointer; z-index: 1000; box-shadow: 0 4px 15px rgba(0,0,0,0.4); transition: 0.3s; }
        #btn-biblioteca:hover { transform: scale(1.1) rotate(10deg); background: #2563eb; }

        .grid { display: grid; grid-template-columns: repeat(18, 1fr); gap: 4px; padding: 20px; max-width: 1400px; margin: auto; }
        .cell { height: 65px; display: flex; flex-direction: column; justify-content: center; align-items: center; border-radius: 4px; cursor: pointer; font-size: 11px; border: 1px solid rgba(255,255,255,0.05); transition: 0.2s; }
        .cell:hover { transform: scale(1.3); z-index: 10; box-shadow: 0 0 20px rgba(255,255,255,0.5); border: 1px solid white; }
        .cell b { font-size: 14px; }

        /* Colores por Categoría */
        .alcalino { background: #ef4444; } .alcalinoterreo { background: #f97316; } .transicion { background: #3b82f6; }
        .post { background: #60a5fa; } .metaloide { background: #a78bfa; } .no-metal { background: #22c55e; }
        .halogeno { background: #84cc16; } .gasnoble { background: #06b6d4; } .lantanido { background: #e879f9; } .actinido { background: #f472b6; }

        #panel { background: #1e293b; padding: 30px; margin: 20px auto; border-radius: 15px; max-width: 900px; border-left: 8px solid #3b82f6; box-shadow: 0 10px 30px rgba(0,0,0,0.5); }
        #atom { width: 100%; height: 450px; background: #0f172a; border-radius: 12px; margin-top: 20px; border: 1px solid #334155; }

        /* Estilos del Modal */
        .modal { display: none; position: fixed; z-index: 2000; left: 0; top: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.9); backdrop-filter: blur(8px); }
        .modal-content { background: #1e293b; margin: 3% auto; padding: 30px; border-radius: 20px; width: 85%; max-width: 850px; height: 80vh; display: flex; flex-direction: column; }
        .tab-menu { display: flex; gap: 10px; border-bottom: 2px solid #334155; margin-bottom: 20px; }
        .tab-btn { background: none; border: none; color: #94a3b8; padding: 12px 25px; cursor: pointer; font-weight: bold; font-size: 16px; transition: 0.3s; }
        .tab-btn.active { color: #3b82f6; border-bottom: 3px solid #3b82f6; }
        .tab-content { display: none; overflow-y: auto; flex-grow: 1; line-height: 1.7; padding-right: 15px; }
        .close-modal { float: right; font-size: 35px; cursor: pointer; color: #94a3b8; }
        .close-modal:hover { color: #f87171; }
        li { margin-bottom: 10px; }
    </style>
</head>
<body>

<div id="btn-biblioteca">📖</div>

<header>
    <input id="search" placeholder="Busca elementos por nombre o símbolo (ej: Oro, Ag)...">
</header>

<div id="grid" class="grid"></div>

<div id="panel">
    <h2 style="text-align:center; color:#94a3b8; font-weight: 300;">Selecciona un elemento para visualizar su átomo y propiedades</h2>
</div>

<div id="modal-info" class="modal">
    <div class="modal-content">
        <span class="close-modal" id="close">&times;</span>
        <h2 style="color:white; margin-top:0;">📚 Biblioteca de Ciencias Aplicadas</h2>
        
        <div class="tab-menu">
            <button class="tab-btn active" onclick="openTab(event, 'fisica')">Física</button>
            <button class="tab-btn" onclick="openTab(event, 'quimica')">Química</button>
            <button class="tab-btn" onclick="openTab(event, 'biologia')">Biología</button>
        </div>

        <div id="fisica" class="tab-content" style="display:block;">
            <h3 style="color: #60a5fa;">⚛️ Física Atómica</h3>
            <p>La física estudia las leyes fundamentales que rigen el comportamiento de los átomos:</p>
            <ul>
                <li><b>Modelo Atómico:</b> Los electrones se distribuyen en niveles de energía. Como ves en el simulador, el número de electrones define la identidad del elemento.</li>
                <li><b>Mecánica Cuántica:</b> Introduce el concepto de orbitales, regiones donde existe la mayor probabilidad de encontrar un electrón.</li>
                <li><b>Núcleo Atómico:</b> Compuesto por protones (carga positiva) y neutrones (sin carga). La estabilidad del núcleo depende de la Fuerza Nuclear Fuerte.</li>
            </ul>
        </div>

        <div id="quimica" class="tab-content">
            <h3 style="color: #22c55e;">🧪 Química y Enlaces</h3>
            <p>La química se centra en cómo los elementos interactúan y se transforman:</p>
            <ul>
                <li><b>Valencia:</b> Los electrones en la última capa (capa de valencia) son los responsables de crear enlaces con otros átomos.</li>
                <li><b>Electronegatividad:</b> Capacidad de un átomo para atraer electrones hacia sí. Es crucial para entender si un enlace será iónico o covalente.</li>
                <li><b>Ley del Octeto:</b> La tendencia de los átomos a completar 8 electrones en su última capa para alcanzar la estabilidad de un gas noble.</li>
            </ul>
        </div>

        <div id="biologia" class="tab-content">
            <h3 style="color: #e879f9;">🧬 Bioquímica</h3>
            <p>La biología depende de la química de los elementos para sustentar la vida:</p>
            <ul>
                <li><b>CHONPS:</b> Los seis elementos principales (Carbono, Hidrógeno, Oxígeno, Nitrógeno, Fósforo y Azufre) que forman las proteínas, lípidos y el ADN.</li>
                <li><b>Metales Vitales:</b> El Magnesio ($Mg$) es el corazón de la clorofila en plantas, y el Hierro ($Fe$) es el núcleo de la hemoglobina en humanos.</li>
                <li><b>Homeostasis:</b> El equilibrio de electrolitos (Sodio, Potasio, Calcio) permite que tus neuronas envíen señales eléctricas.</li>
            </ul>
        </div>
    </div>
</div>

<script>
// --- 1. BASE DE DATOS COMPLETA (118 ELEMENTOS) ---
const datos = {
    "H":{name:"Hidrógeno",mass:1.008,config:"1s1",en:2.20},"He":{name:"Helio",mass:4.002,config:"1s2",en:null},
    "Li":{name:"Litio",mass:6.94,config:"[He] 2s1",en:0.98},"Be":{name:"Berilio",mass:9.012,config:"[He] 2s2",en:1.57},
    "B":{name:"Boro",mass:10.81,config:"[He] 2s2 2p1",en:2.04},"C":{name:"Carbono",mass:12.011,config:"[He] 2s2 2p2",en:2.55},
    "N":{name:"Nitrógeno",mass:14.007,config:"[He] 2s2 2p3",en:3.04},"O":{name:"Oxígeno",mass:15.999,config:"[He] 2s2 2p4",en:3.44},
    "F":{name:"Flúor",mass:18.998,config:"[He] 2s2 2p5",en:3.98},"Ne":{name:"Neón",mass:20.180,config:"[He] 2s2 2p6",en:null},
    "Na":{name:"Sodio",mass:22.990,config:"[Ne] 3s1",en:0.93},"Mg":{name:"Magnesio",mass:24.305,config:"[Ne] 3s2",en:1.31},
    "Al":{name:"Aluminio",mass:26.982,config:"[Ne] 3s2 3p1",en:1.61},"Si":{name:"Silicio",mass:28.085,config:"[Ne] 3s2 3p2",en:1.90},
    "P":{name:"Fósforo",mass:30.974,config:"[Ne] 3s2 3p3",en:2.19},"S":{name:"Azufre",mass:32.06,config:"[Ne] 3s2 3p4",en:2.58},
    "Cl":{name:"Cloro",mass:35.45,config:"[Ne] 3s2 3p5",en:3.16},"Ar":{name:"Argón",mass:39.948,config:"[Ne] 3s2 3p6",en:null},
    "K":{name:"Potasio",mass:39.098,config:"[Ar] 4s1",en:0.82},"Ca":{name:"Calcio",mass:40.078,config:"[Ar] 4s2",en:1.00},
    "Sc":{name:"Escandio",mass:44.956,config:"[Ar] 3d1 4s2",en:1.36},"Ti":{name:"Titanio",mass:47.867,config:"[Ar] 3d2 4s2",en:1.54},
    "V":{name:"Vanadio",mass:50.942,config:"[Ar] 3d3 4s2",en:1.63},"Cr":{name:"Cromo",mass:51.996,config:"[Ar] 3d5 4s1",en:1.66},
    "Mn":{name:"Manganeso",mass:54.938,config:"[Ar] 3d5 4s2",en:1.55},"Fe":{name:"Hierro",mass:55.845,config:"[Ar] 3d6 4s2",en:1.83},
    "Co":{name:"Cobalto",mass:58.933,config:"[Ar] 3d7 4s2",en:1.88},"Ni":{name:"Níquel",mass:58.693,config:"[Ar] 3d8 4s2",en:1.91},
    "Cu":{name:"Cobre",mass:63.546,config:"[Ar] 3d10 4s1",en:1.90},"Zn":{name:"Zinc",mass:65.38,config:"[Ar] 3d10 4s2",en:1.65},
    "Ga":{name:"Galio",mass:69.723,config:"[Ar] 3d10 4s2 4p1",en:1.81},"Ge":{name:"Germanio",mass:72.63,config:"[Ar] 3d10 4s2 4p2",en:2.01},
    "As":{name:"Arsénico",mass:74.922,config:"[Ar] 3d10 4s2 4p3",en:2.18},"Se":{name:"Selenio",mass:78.971,config:"[Ar] 3d10 4s2 4p4",en:2.55},
    "Br":{name:"Bromo",mass:79.904,config:"[Ar] 3d10 4s2 4p5",en:2.96},"Kr":{name:"Kriptón",mass:83.798,config:"[Ar] 3d10 4s2 4p6",en:3.00},
    "Rb":{name:"Rubidio",mass:85.468,config:"[Kr] 5s1",en:0.82},"Sr":{name:"Estroncio",mass:87.62,config:"[Kr] 5s2",en:0.95},
    "Y":{name:"Itrio",mass:88.906,config:"[Kr] 4d1 5s2",en:1.22},"Zr":{name:"Zirconio",mass:91.224,config:"[Kr] 4d2 5s2",en:1.33},
    "Nb":{name:"Niobio",mass:92.906,config:"[Kr] 4d4 5s1",en:1.6},"Mo":{name:"Molibdeno",mass:95.95,config:"[Kr] 4d5 5s1",en:2.16},
    "Tc":{name:"Tecnecio",mass:98,config:"[Kr] 4d5 5s2",en:1.9},"Ru":{name:"Rutenio",mass:101.07,config:"[Kr] 4d7 5s1",en:2.2},
    "Rh":{name:"Rodio",mass:102.91,config:"[Kr] 4d8 5s1",en:2.28},"Pd":{name:"Paladio",mass:106.42,config:"[Kr] 4d10",en:2.20},
    "Ag":{name:"Plata",mass:107.87,config:"[Kr] 4d10 5s1",en:1.93},"Cd":{name:"Cadmio",mass:112.41,config:"[Kr] 4d10 5s2",en:1.69},
    "In":{name:"Indio",mass:114.82,config:"[Kr] 4d10 5s2 5p1",en:1.78},"Sn":{name:"Estaño",mass:118.71,config:"[Kr] 4d10 5s2 5p2",en:1.96},
    "Sb":{name:"Antimonio",mass:121.76,config:"[Kr] 4d10 5s2 5p3",en:2.05},"Te":{name:"Telurio",mass:127.60,config:"[Kr] 4d10 5s2 5p4",en:2.1},
    "I":{name:"Yodo",mass:126.90,config:"[Kr] 4d10 5s2 5p5",en:2.66},"Xe":{name:"Xenón",mass:131.29,config:"[Kr] 4d10 5s2 5p6",en:2.6},
    "Cs":{name:"Cesio",mass:132.91,config:"[Xe] 6s1",en:0.79},"Ba":{name:"Bario",mass:137.33,config:"[Xe] 6s2",en:0.89},
    "La":{name:"Lantano",mass:138.91,config:"[Xe] 5d1 6s2",en:1.1},"Ce":{name:"Cerio",mass:140.12,config:"[Xe] 4f1 5d1 6s2",en:1.12},
    "Pr":{name:"Praseodimio",mass:140.91,config:"[Xe] 4f3 6s2",en:1.13},"Nd":{name:"Neodimio",mass:144.24,config:"[Xe] 4f4 6s2",en:1.14},
    "Pm":{name:"Prometio",mass:145,config:"[Xe] 4f5 6s2",en:1.13},"Sm":{name:"Samario",mass:150.36,config:"[Xe] 4f6 6s2",en:1.17},
    "Eu":{name:"Europio",mass:151.96,config:"[Xe] 4f7 6s2",en:1.2},"Gd":{name:"Gadolinio",mass:157.25,config:"[Xe] 4f7 5d1 6s2",en:1.2},
    "Tb":{name:"Terbio",mass:158.93,config:"[Xe] 4f9 6s2",en:1.1},"Dy":{name:"Disprosio",mass:162.50,config:"[Xe] 4f10 6s2",en:1.22},
    "Ho":{name:"Holmio",mass:164.93,config:"[Xe] 4f11 6s2",en:1.23},"Er":{name:"Erbio",mass:167.26,config:"[Xe] 4f12 6s2",en:1.24},
    "Tm":{name:"Tulio",mass:168.93,config:"[Xe] 4f13 6s2",en:1.25},"Yb":{name:"Iterbio",mass:173.05,config:"[Xe] 4f14 6s2",en:1.1},
    "Lu":{name:"Lutecio",mass:174.97,config:"[Xe] 4f14 5d1 6s2",en:1.27},"Hf":{name:"Hafnio",mass:178.49,config:"[Xe] 4f14 5d2 6s2",en:1.3},
    "Ta":{name:"Tantalio",mass:180.95,config:"[Xe] 4f14 5d3 6s2",en:1.5},"W":{name:"Wolframio",mass:183.84,config:"[Xe] 4f14 5d4 6s2",en:2.36},
    "Re":{name:"Renio",mass:186.21,config:"[Xe] 4f14 5d5 6s2",en:1.9},"Os":{name:"Osmio",mass:190.23,config:"[Xe] 4f14 5d6 6s2",en:2.2},
    "Ir":{name:"Iridio",mass:192.22,config:"[Xe] 4f14 5d7 6s2",en:2.2},"Pt":{name:"Platino",mass:195.08,config:"[Xe] 4f14 5d9 6s1",en:2.28},
    "Au":{name:"Oro",mass:196.97,config:"[Xe] 4f14 5d10 6s1",en:2.54},"Hg":{name:"Mercurio",mass:200.59,config:"[Xe] 4f14 5d10 6s2",en:2.00},
    "Tl":{name:"Talio",mass:204.38,config:"[Xe] 4f14 5d10 6s2 6p1",en:1.62},"Pb":{name:"Plomo",mass:207.2,config:"[Xe] 4f14 5d10 6s2 6p2",en:2.33},
    "Bi":{name:"Bismuto",mass:208.98,config:"[Xe] 4f14 5d10 6s2 6p3",en:2.02},"Po":{name:"Polonio",mass:209,config:"[Xe] 4f14 5d10 6s2 6p4",en:2.0},
    "At":{name:"Astato",mass:210,config:"[Xe] 4f14 5d10 6s2 6p5",en:2.2},"Rn":{name:"Radón",mass:222,config:"[Xe] 4f14 5d10 6s2 6p6",en:2.2},
    "Fr":{name:"Francio",mass:223,config:"[Rn] 7s1",en:0.7},"Ra":{name:"Radio",mass:226,config:"[Rn] 7s2",en:0.9},
    "Ac":{name:"Actinio",mass:227,config:"[Rn] 6d1 7s2",en:1.1},"Th":{name:"Torio",mass:232.04,config:"[Rn] 6d2 7s2",en:1.3},
    "Pa":{name:"Protactinio",mass:231.04,config:"[Rn] 5f2 6d1 7s2",en:1.5},"U":{name:"Uranio",mass:238.03,config:"[Rn] 5f3 6d1 7s2",en:1.38},
    "Np":{name:"Neptunio",mass:237,config:"[Rn] 5f4 6d1 7s2",en:1.36},"Pu":{name:"Plutonio",mass:244,config:"[Rn] 5f6 7s2",en:1.28},
    "Am":{name:"Americio",mass:243,config:"[Rn] 5f7 7s2",en:1.13},"Cm":{name:"Curio",mass:247,config:"[Rn] 5f7 6d1 7s2",en:1.28},
    "Bk":{name:"Berkelio",mass:247,config:"[Rn] 5f9 7s2",en:1.3},"Cf":{name:"Californio",mass:251,config:"[Rn] 5f10 7s2",en:1.3},
    "Es":{name:"Einsteinio",mass:252,config:"[Rn] 5f11 7s2",en:1.3},"Fm":{name:"Fermio",mass:257,config:"[Rn] 5f12 7s2",en:1.3},
    "Md":{name:"Mendelevio",mass:258,config:"[Rn] 5f13 7s2",en:1.3},"No":{name:"Nobelio",mass:259,config:"[Rn] 5f14 7s2",en:1.3},
    "Lr":{name:"Laurencio",mass:262,config:"[Rn] 5f14 7s2 7p1",en:1.3},
    "Rf":{name:"Rutherfordio",mass:267,config:"[Rn] 5f14 6d2 7s2",en:null},"Db":{name:"Dubnio",mass:270,config:"[Rn] 5f14 6d3 7s2",en:null},
    "Sg":{name:"Seaborgio",mass:271,config:"[Rn] 5f14 6d4 7s2",en:null},"Bh":{name:"Bohrio",mass:270,config:"[Rn] 5f14 6d5 7s2",en:null},
    "Hs":{name:"Hassio",mass:277,config:"[Rn] 5f14 6d6 7s2",en:null},"Mt":{name:"Meitnerio",mass:278,config:"[Rn] 5f14 6d7 7s2",en:null},
    "Ds":{name:"Darmstadtio",mass:281,config:"[Rn] 5f14 6d8 7s2",en:null},"Rg":{name:"Roentgenio",mass:282,config:"[Rn] 5f14 6d9 7s2",en:null},
    "Cn":{name:"Copernicio",mass:285,config:"[Rn] 5f14 6d10 7s2",en:null},"Nh":{name:"Nihonio",mass:286,config:"[Rn] 5f14 6d10 7s2 7p1",en:null},
    "Fl":{name:"Flerovio",mass:289,config:"[Rn] 5f14 6d10 7s2 7p2",en:null},"Mc":{name:"Moscovio",mass:290,config:"[Rn] 5f14 6d10 7s2 7p3",en:null},
    "Lv":{name:"Livermorio",mass:293,config:"[Rn] 5f14 6d10 7s2 7p4",en:null},"Ts":{name:"Teneso",mass:294,config:"[Rn] 5f14 6d10 7s2 7p5",en:null},
    "Og":{name:"Oganesón",mass:294,config:"[Rn] 5f14 6d10 7s2 7p6",en:null}
};

const elementos = [
    [1,"H",1,1,"no-metal"],[2,"He",18,1,"gasnoble"],[3,"Li",1,2,"alcalino"],[4,"Be",2,2,"alcalinoterreo"],[5,"B",13,2,"metaloide"],[6,"C",14,2,"no-metal"],[7,"N",15,2,"no-metal"],[8,"O",16,2,"no-metal"],[9,"F",17,2,"halogeno"],[10,"Ne",18,2,"gasnoble"],[11,"Na",1,3,"alcalino"],[12,"Mg",2,3,"alcalinoterreo"],[13,"Al",13,3,"post"],[14,"Si",14,3,"metaloide"],[15,"P",15,3,"no-metal"],[16,"S",16,3,"no-metal"],[17,"Cl",17,3,"halogeno"],[18,"Ar",18,3,"gasnoble"],[19,"K",1,4,"alcalino"],[20,"Ca",2,4,"alcalinoterreo"],[21,"Sc",3,4,"transicion"],[22,"Ti",4,4,"transicion"],[23,"V",5,4,"transicion"],[24,"Cr",6,4,"transicion"],[25,"Mn",7,4,"transicion"],[26,"Fe",8,4,"transicion"],[27,"Co",9,4,"transicion"],[28,"Ni",10,4,"transicion"],[29,"Cu",11,4,"transicion"],[30,"Zn",12,4,"transicion"],[31,"Ga",13,4,"post"],[32,"Ge",14,4,"metaloide"],[33,"As",15,4,"metaloide"],[34,"Se",16,4,"no-metal"],[35,"Br",17,4,"halogeno"],[36,"Kr",18,4,"gasnoble"],[37,"Rb",1,5,"alcalino"],[38,"Sr",2,5,"alcalinoterreo"],[39,"Y",3,5,"transicion"],[40,"Zr",4,5,"transicion"],[41,"Nb",5,5,"transicion"],[42,"Mo",6,5,"transicion"],[43,"Tc",7,5,"transicion"],[44,"Ru",8,5,"transicion"],[45,"Rh",9,5,"transicion"],[46,"Pd",10,5,"transicion"],[47,"Ag",11,5,"transicion"],[48,"Cd",12,5,"transicion"],[49,"In",13,5,"post"],[50,"Sn",14,5,"post"],[51,"Sb",15,5,"metaloide"],[52,"Te",16,5,"metaloide"],[53,"I",17,5,"halogeno"],[54,"Xe",18,5,"gasnoble"],[55,"Cs",1,6,"alcalino"],[56,"Ba",2,6,"alcalinoterreo"],[57,"La",3,8,"lantanido"],[58,"Ce",4,8,"lantanido"],[59,"Pr",5,8,"lantanido"],[60,"Nd",6,8,"lantanido"],[61,"Pm",7,8,"lantanido"],[62,"Sm",8,8,"lantanido"],[63,"Eu",9,8,"lantanido"],[64,"Gd",10,8,"lantanido"],[65,"Tb",11,8,"lantanido"],[66,"Dy",12,8,"lantanido"],[67,"Ho",13,8,"lantanido"],[68,"Er",14,8,"lantanido"],[69,"Tm",15,8,"lantanido"],[70,"Yb",16,8,"lantanido"],[71,"Lu",17,8,"lantanido"],[72,"Hf",4,6,"transicion"],[73,"Ta",5,6,"transicion"],[74,"W",6,6,"transicion"],[75,"Re",7,6,"transicion"],[76,"Os",8,6,"transicion"],[77,"Ir",9,6,"transicion"],[78,"Pt",10,6,"transicion"],[79,"Au",11,6,"transicion"],[80,"Hg",12,6,"transicion"],[81,"Tl",13,6,"post"],[82,"Pb",14,6,"post"],[83,"Bi",15,6,"post"],[84,"Po",16,6,"metaloide"],[85,"At",17,6,"halogeno"],[86,"Rn",18,6,"gasnoble"],[87,"Fr",1,7,"alcalino"],[88,"Ra",2,7,"alcalinoterreo"],[89,"Ac",3,9,"actinido"],[90,"Th",4,9,"actinido"],[91,"Pa",5,9,"actinido"],[92,"U",6,9,"actinido"],[93,"Np",7,9,"actinido"],[94,"Pu",8,9,"actinido"],[95,"Am",9,9,"actinido"],[96,"Cm",10,9,"actinido"],[97,"Bk",11,9,"actinido"],[98,"Cf",12,9,"actinido"],[99,"Es",13,9,"actinido"],[100,"Fm",14,9,"actinido"],[101,"Md",15,9,"actinido"],[102,"No",16,9,"actinido"],[103,"Lr",17,9,"actinido"],[104,"Rf",4,7,"transicion"],[105,"Db",5,7,"transicion"],[106,"Sg",6,7,"transicion"],[107,"Bh",7,7,"transicion"],[108,"Hs",8,7,"transicion"],[109,"Mt",9,7,"transicion"],[110,"Ds",10,7,"transicion"],[111,"Rg",11,7,"transicion"],[112,"Cn",12,7,"transicion"],[113,"Nh",13,7,"post"],[114,"Fl",14,7,"post"],[115,"Mc",15,7,"post"],[116,"Lv",16,7,"post"],[117,"Ts",17,7,"halogeno"],[118,"Og",18,7,"gasnoble"]
];

// --- 2. LÓGICA DE APLICACIÓN ---
let animationId = null;

function renderTabla(lista) {
    const grid = document.getElementById("grid");
    grid.innerHTML = "";
    lista.forEach(e => {
        const [num, sym, col, row, cat] = e;
        let div = document.createElement("div");
        div.className = "cell " + cat;
        div.style.gridColumn = col;
        div.style.gridRow = row;
        div.innerHTML = `${num}<br><b>${sym}</b>`;
        div.onclick = () => mostrarPanel(sym, num);
        grid.appendChild(div);
    });
}

function mostrarPanel(sym, num) {
    const d = datos[sym] || { name: sym, mass: "—", config: "—", en: "—" };
    if (animationId) cancelAnimationFrame(animationId);

    document.getElementById("panel").innerHTML = `
        <h2 style="color:#60a5fa; margin:0 0 15px 0; font-size: 28px;">${d.name} (${sym})</h2>
        <div style="line-height: 1.8; font-size: 16px;">
            <p><b>Número atómico:</b> ${num}</p>
            <p><b>Masa atómica:</b> ${d.mass} u</p>
            <p><b>Configuración:</b> <span style="color:#fbbf24;">${d.config}</span></p>
            <p><b>Electronegatividad:</b> ${d.en || "N/A"}</p>
        </div>
        <div id="atom"></div>
    `;
    iniciarAtomo3D(num);
}

function iniciarAtomo3D(num) {
    const container = document.getElementById("atom");
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, container.clientWidth / 450, 0.1, 1000);
    camera.position.set(0, 5, 12);

    const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
    renderer.setSize(container.clientWidth, 450);
    container.appendChild(renderer.domElement);

    const controls = new THREE.OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;

    // Núcleo central
    const core = new THREE.Mesh(new THREE.SphereGeometry(1.3, 32, 32), new THREE.MeshStandardMaterial({ color: 0xff4444, emissive: 0x220000 }));
    scene.add(core);
    scene.add(new THREE.AmbientLight(0xffffff, 0.7));
    const pointLight = new THREE.PointLight(0xffffff, 1.5);
    pointLight.position.set(10, 10, 10);
    scene.add(pointLight);

    // Distribución de electrones por capas
    const capas = [2, 8, 18, 32, 32, 18, 8];
    let rest = num;
    let dist = 3;

    capas.forEach((max, nivel) => {
        let eEnCapa = Math.min(rest, max);
        if (eEnCapa <= 0) return;

        // Anillo de órbita
        const ring = new THREE.Mesh(new THREE.RingGeometry(dist - 0.05, dist + 0.05, 100), new THREE.MeshBasicMaterial({ color: 0x00ffff, opacity: 0.15, transparent: true, side: THREE.DoubleSide }));
        ring.rotation.x = Math.PI / 2;
        scene.add(ring);

        // Electrones individuales
        for (let i = 0; i < eEnCapa; i++) {
            const electron = new THREE.Mesh(new THREE.SphereGeometry(0.2, 16, 16), new THREE.MeshStandardMaterial({ color: 0x00ffff, emissive: 0x00ffff }));
            let angulo = (i / eEnCapa) * Math.PI * 2;
            electron.userData = { dist, angulo, v: 0.02 / (nivel + 1) };
            scene.add(electron);
        }
        rest -= eEnCapa;
        dist += 1.2;
    });

    function anim() {
        animationId = requestAnimationFrame(anim);
        scene.children.forEach(o => {
            if (o.userData.dist) {
                o.userData.angulo += o.userData.v;
                o.position.x = Math.cos(o.userData.angulo) * o.userData.dist;
                o.position.z = Math.sin(o.userData.angulo) * o.userData.dist;
            }
        });
        controls.update();
        renderer.render(scene, camera);
    }
    anim();
}

// --- 3. FUNCIONES DE UI ---
const modal = document.getElementById("modal-info");
document.getElementById("btn-biblioteca").onclick = () => modal.style.display = "block";
document.getElementById("close").onclick = () => modal.style.display = "none";

function openTab(evt, name) {
    let contents = document.getElementsByClassName("tab-content");
    for (let c of contents) c.style.display = "none";
    let btns = document.getElementsByClassName("tab-btn");
    for (let b of btns) b.classList.remove("active");
    document.getElementById(name).style.display = "block";
    evt.currentTarget.classList.add("active");
}

document.getElementById("search").oninput = e => {
    let q = e.target.value.toLowerCase();
    renderTabla(elementos.filter(el => el[1].toLowerCase().includes(q) || datos[el[1]]?.name.toLowerCase().includes(q)));
};

// Inicializar
renderTabla(elementos);
</script>
</body>
</html>
