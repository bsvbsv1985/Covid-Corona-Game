
<!DOCTYPE html>
<html>
    <head>
    <style>

body {
     background-color:black;     
     margin:0;padding:0;
     font-family: 'Fjalla One';
     user-select: none;
    -webkit-user-select : none;
    overflow:hidden;
}
#beginning{
    position:absolute;
    top:0;
    width:100%;
    height:100%;
    background-color:teal;
    z-index:3;
    color:yellow;
    text-align:center;

    display:flex;
    flex-direction:column;
    justify-content:space-around;
    align-items:center;
}
h1{
    padding:10px;
    border:5px solid red;
    width:310px;
    font-size:1.5em;
    
}
.btnplay{
    width:100px;
    height:30px;
    border:3px solid red;
    display:flex;
    justify-content:center;
    align-items:center;
    border-radius:24px;
    transform:skew(-45deg);
    animation:movebtn 1s linear infinite;
}
@keyframes movebtn {50%{transform:skew(15deg);}}
#finish{
    position:absolute;
    top:0;
    width:100%;
    height:100%;
    background-color:teal;
    z-index:3;
    color:gold;
    text-align:center;
    display:none;
    flex-direction:column;
    justify-content:space-around;
    align-items:center;
}
h2{
    padding:10px;
    border:5px dashed white;
    width:200px;
    font-size:1.5em;
}
canvas{
    background-color:black;
    position:absolute;
    z-index:1;
}
#infos{
    position:absolute;
    top:1px;
    right:50px;
    width:250px;
    height:30px;
    display:flex;
    font-style:yellow;
    flex-direction:inline;
    justify-content:space-around;
    z-index:2;
    color:gold;
    font-size:1em;
    
}
.info{
    width:100%;
    display:flex;
    align-items:center;
    justify-content:center;
    border:1px solid green;
    
}
#infosval{
    position:absolute;
    top:30px;
    right:50px;
    width:250px;
    height:30px;
    display:flex;
    flex-direction:inline;
    justify-content:space-around;
    z-index:2;
    color:yellow;
    font-size:0.8em;
}

#helicopter{
    position:relative  ;
    left:5px;
    width:100px;    
    height:50px;
    z-index:2;
}
#helicopterbody{
    position:absolute ;
    top:10px;
    left:50px;
    width:50px;
    height:30px;
    border-radius:0 50px 30px 30px;
    background-color:#CF0D0D;
    overflow:hidden;
}
#helicopterwindow{
    position:absolute; 
    top:0;
    left:30px;
    width:20px;
    height:20px;
    background-color:#31B2F5;
}
#helicopterwindow:before{
    position:absolute;
    content:'';
    top:4px;
    left:-8px;
    width:20px;
    height:20px;
    border-radius:50%;
    border:1.5px solid transparent;
    border-right:1.5px solid rgba(255,255,255,0.5);
    transform:rotate(-45deg);
}
#cross{
    position:absolute;
    top:10px;
    left:10px;
    width:12px;
    height:2px;
    background-color:white;
}
#cross:before{
    position:absolute;
    content:'';
    top:0;
    left:0;
    width:12px;
    height:2px;
    transform:rotate(90deg);
    background-color:white;
}
#helicoptertail{
    position:absolute;
    top:10px;
    left:12px;
    width:40px;
    height:8px;
    background-color:#CF0D0D;
}
#helicopterrotor{
    position:absolute;
    top:0px;
    left:40px;
    width:60px;
    height:2px;
    background-color:white;
    border-radius:2px;
    animation:move 0.5s linear infinite;
}
@keyframes move {0%{transform:rotateY(360deg);}}
#helicopterfeet2{
    position:absolute;
    top:3px;
    left:70px;
    width:3px;
    height:10px;
    background-color:white;
}
#helicopterfeet1{
    position:absolute;
    top:46px;
    left:55px;
    width:40px;
    height:4px;
    border-radius:2px;
    background-color:white;
}
#helicopterfeet1:before{
    position:absolute;
    content:'';
    top:-7px;
    left:12px;
    width:3px;
    height:10px;
    background-color:white;
}
#helicopterfeet1:after{
    position:absolute;
    content:'';
    top:-7px;
    left:28px;
    width:3px;
    height:10px;
    background-color:white;
}
#helicopterrotorback{
    position:absolute;
    top:12.5px;
    left:4px;
    width:17px;
    height:3px;
    background-color:white;
    animation: move2 0.5s linear infinite;
}
@keyframes move2 {0%{transform:rotate(-360deg);}}
#helicopterrotorback:before{
    position:absolute;
    content:'';
    top:0;
    left:0;
    transform:rotate(90deg);
    width:17px;
    height:3px;
    background-color:white;
}
#commands{
    position:absolute;
    bottom:0;
    width:100%;
    display:flex;
    flex-direction:inline;
    justify-content:space-around;
    align-items:center;
    color:yellow;
     z-index:2;
}
.btn{ 
text-align:center ;
    width:100%;
    height:50px;
    display:inline ;
    
    justify-content:center;
    box-shadow:inset 0 -2px 3px black,inset 0 2px 3px black;
}
.down{ 
    background-color:green;
}
.shoot{
    background-color:red;
    
}
.up{
    background-color:green;
}</style>
<script>
let W, H;
let c, ctx;

let helico, btns;
let posX, posY;
let speed, speedX;

let virusImg, intervalVirus;
let virus = [];
let shoots = [];

let lost, kill, life;

let cptTime, intervalTime;

const init = () => {
    W = window.innerWidth;
    H = window.innerHeight;
    
    c = document.getElementById("canvas");
    c.width = W;
    c.height = H;
    ctx = c.getContext("2d");
    
    helico = document.getElementById('helicopter');
    posY = W/2;
    speed = 0;
    speedX = 5;
    helico.style.top = posY + "px";
    
    virusImg = new Image();
    virusImg.src = "https://i.ibb.co/McZ36tZ/virus.png";
    
    lost = 0;
    kill = 0;
    life = 100;
    
    cptTime = 100;
    intervalTime = setInterval(getTime, 1000);
    
    // phone
    btns = document.getElementsByClassName('btn');
    btns[0].addEventListener("touchstart", () => up() );
    btns[0].addEventListener("touchend", () => stop() );
    btns[1].addEventListener("touchstart", () => shoot() );
    btns[2].addEventListener("touchstart", () => down() );
    btns[2].addEventListener("touchend", () => stop() );
    
    //keyboard
    var mapKeyPress = {};
    let stateShoot = false;
    document.onkeydown = (e) => {
        e = e || window.event;
        mapKeyPress[e.keyCode] = true;
        if (e.keyCode == '40') down();
        else if (e.keyCode == '38')  up();
        else if(!stateShoot){
            shoot();
            stateShoot = true;
        }
    };
    document.onkeyup =(e) => {
        e = e || window.event;
        mapKeyPress[e.keyCode] = false;
        if (mapKeyPress[40] == true) down();
        else if (mapKeyPress[38] == true)  up();
        else stop();
        stateShoot = false;
    };
    
    animate();    
};

const random = (max=1, min=0) => Math.random() * (max - min) + min;
const clear = () => ctx.clearRect(0, 0, W, H);
const updateLost = () => document.getElementsByClassName('lost')[0].innerText = lost;
const updateKill = () => document.getElementsByClassName('kill')[0].innerText = kill;
const updateLife = () => document.getElementsByClassName('life')[0].innerText = life + "%";

const getTime = () => {
    cptTime--;
    document.getElementsByClassName('time')[0].innerText = cptTime;
};

const down = () => {
    speed = 2;
    helico.style.transform = "rotate(3deg)";
};

const up = () => {
    speed =-4;
    helico.style.transform = "rotate(-3deg)";
};

const stop = () => {
    speed = 0;
    helico.style.transform = "rotate(0)";
};

const shoot = () => shoots.push(new Shoot(100, posY+25));

const checkHelico = () => {
    if(posY>0){
        posY>H-105 && speed>=0 ? speed = 0:  posY += 1;
        posY += speed;
    }
    else posY += 1;
    if(posY>H-105) helico.style.transform = "rotate(0)";
    
    helico.style.top = posY + "px";
    helico.style.left = speedX + "px";
};

const checkVirus = () => {
    for(var i = virus.length - 1; i >= 0; i--){
        virus[i].update();
        if(virus[i].x<0){
            virus.splice(i, 1);
            lost++;
            updateLost();
        }
        else if(virus[i].x<100&&virus[i].y+30>posY&&virus[i].y<posY+50){
            virus.splice(i, 1);
            life -= 20;
            lost++;
            updateLife();
            updateLost();
            signalDead();
        }
    }
};

const checkShoot = () => {
    for(var i = shoots.length - 1; i >= 0; i--){
        shoots[i].update();
        if(shoots[i].x>W) shoots.splice(i, 1);
    }
};

const newVirus = () => virus.push(new Virus());

const end = () => {
    clearInterval(intervalTime);
    shoots = [];
    virus = [];
    speedX += 5;
    if(life>0&&speedX<=10){
        setTimeout(function() { 
            document.getElementById('finish').style.display = 'flex';
            document.getElementById('state').innerText = 'CONGRATS !';
            document.getElementById('cptkill').innerText = kill + " KILLED";
            document.getElementById('cptlost').innerText = lost + " LOST";
            }, 2000);
        
    }
    else if(life<=0){
        document.getElementById('finish').style.display = 'flex';
        document.getElementById('state').innerText = 'YOU LOSE IN ' + (120-cptTime) + " SECONDS";
        document.getElementById('cptkill').innerText = kill + " KILLED";
        document.getElementById('cptlost').innerText = lost + " LOST";
    }
};

const checkLife = () =>  {
    if(life<=0) end();
};

const checkTimer = () =>  {
    if(cptTime<=0) end();
};

const start = () => {
    document.getElementById('beginning').style.display = 'none';
    intervalVirus = setInterval(newVirus, 1000);
};

const lose = () => {
    clearInterval(intervalTime);
    shoots = [];
    virus = [];
    document.getElementById('finish').style.display = 'flex';
};


const retry = () =>{
    lost = 0;
    kill = 0;
    life = 100;
    cptTime = 120;
    posY = W/2;
    speed = 0;
    speedX = 5;
    geTime();
    intervalTime = setInterval(getTime, 1000);
    updateLost();
    updateKill();
    updateLife();
    
    document.getElementById('finish').style.display = 'none';
};

const signalDead = () => {
    canvas.style.backgroundColor = 'red';
    setTimeout(function() { 
        canvas.style.backgroundColor = "transparent";
    }, 100);
};

const animate = () => {
    clear();
    checkVirus();
    checkShoot();
    checkHelico();
    checkLife();
    checkTimer();
    window.requestAnimationFrame(animate);
};

class Virus {
    constructor() {
        this.x = W + ~~random(30);
        this.y = ~~random(H-90, 70);
        this.speed = ~~random(3, 1);
    }
    draw() {
        ctx.beginPath();
        ctx.drawImage(virusImg, this.x, this.y, 30, 30);
    }
    update() {
        this.x -= this.speed;
        this.draw();
    }
}

class Shoot {
    constructor(x, y) {
        this.x = x;
        this.y = y;
        this.speed = 4;
    }
    draw() {
        ctx.beginPath();
        ctx.fillStyle = 'white';
        ctx.arc(this.x, this.y,  3, 0, Math.PI * 2, true);
        ctx.fill();
    }
    update() {
        for(let i = virus.length - 1; i >= 0; i--){
            if(this.x>virus[i].x&&this.x<virus[i].x+30&&this.y>virus[i].y&&this.y<virus[i].y+30){
                virus.splice(i, 1);
                kill++;
                updateKill();
                this.x = W + 100;
            }
        }
        this.x += this.speed;
        this.draw();
    }
}

window.onload = init;
</script>
        <title>Shoot the | CORONAVIRUS by Helicopter</title>
        <link href="https://fonts.googleapis.com/css?family=Fjalla+One&display=swap" rel="stylesheet">
    </head>
    <body>
        
        <canvas id="canvas"></canvas>
        
        <div id="time">100</div>
        
        <div id="infos">
            <div class="info">TIMER</div>
            <div class="info">LIFE</div>
            <div class="info">KILL</div>
            <div class="info">LOST</div>
        </div>
        
        <div id="infosval">
             <div class="info time">1000</div>
             <div class="info life">100%</div>
             <div class="info kill">0</div>
             <div class="info lost">0</div>
        </div>
        
        <div id="helicopter">
            <div id="helicopterrotor"></div>
            <div id="helicopterfeet1"></div>
            <div id="helicopterfeet2"></div>
            <div id="helicopterbody">
                <div id="helicopterwindow"></div>
                <div id="cross"></div>
            </div>
            <div id="helicoptertail"></div>
            <div id="helicopterrotorback"></div>
        </div>
        
        <div id="commands">
            <div class="btn up">UP</div>
            <div class="btn shoot">SHOOT</div>
            <div class="btn down">DOWN</div>
        </div>
        
        <div id="beginning">
            <h1>Shoot the Coronavirus</h1>
            <p>
                <span style="font-size:1.3em">You have 100 seconds</span> <br><br><br>
                Kill as many viruses as possible !<br><br>
                Be careful not to touch the virus !
            </p>
            <div class="btnplay" onClick="start()">PLAY</div>
        </div>
        
        <div id="finish">
                <h2>END &nbsp; GAME</h2>
                <p>
                    <span id="state" style="font-size:1.3em">YOU LOSE IN  seconds</span><br><br><br>
                    <span id="cptkill">5</span><br><br>
                    <span id="cptlost">10</span>
                </p>
                <div class="btnplay" onClick="retry()">PLAY</div>
        </div> 
        
    </body>
</html>
