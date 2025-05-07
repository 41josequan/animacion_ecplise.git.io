<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Animación Eclipse con Cielo y Noche</title>
<style>
  html, body {
    height: 100%;
    margin: 0;
    overflow: hidden;
  }
  .escena {
    position: relative;
    width: 100vw;
    height: 100vh;
    background: linear-gradient(to bottom, #a0d8ff, #d0e8ff);
    overflow: hidden;
    transition: background 1s;
    z-index: 0;
    animation: cambiar-fondo 1s 2s forwards;
  }

  .escena::before {
    content: "";
    position: absolute;
    width: 100%;
    height: 100%;
    background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%"><circle cx="10%" cy="10%" r="40" fill="rgba(255,255,255,0.7)"/><circle cx="30%" cy="5%" r="60" fill="rgba(255,255,255,0.7)"/><circle cx="55%" cy="8%" r="35" fill="rgba(255,255,255,0.7)"/><circle cx="75%" cy="12%" r="50" fill="rgba(255,255,255,0.7)"/><circle cx="90%" cy="7%" r="45" fill="rgba(255,255,255,0.7)"/><circle cx="15%" cy="20%" r="50" fill="rgba(255,255,255,0.7)"/><circle cx="40%" cy="18%" r="35" fill="rgba(255,255,255,0.7)"/><circle cx="70%" cy="22%" r="40" fill="rgba(255,255,255,0.7)"/><circle cx="85%" cy="25%" r="30" fill="rgba(255,255,255,0.7)"/></svg>');
    z-index: 1;
    pointer-events: none;
    animation: cambiar-estrellas 1s 2s forwards;
  }

  @keyframes cambiar-fondo {
    to {
      background: radial-gradient(ellipse at center, #001a3a 0%, #000 100%);
    }
  }

  @keyframes cambiar-estrellas {
    to {
      background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%"><circle cx="10%" cy="10%" r="1" fill="white"/><circle cx="20%" cy="15%" r="1.5" fill="white"/><circle cx="30%" cy="5%" r="1" fill="white"/><circle cx="40%" cy="12%" r="1.2" fill="white"/><circle cx="50%" cy="8%" r="1" fill="white"/><circle cx="60%" cy="20%" r="1.5" fill="white"/><circle cx="70%" cy="15%" r="1" fill="white"/><circle cx="80%" cy="10%" r="1.2" fill="white"/><circle cx="90%" cy="25%" r="1" fill="white"/><circle cx="15%" cy="30%" r="1.2" fill="white"/><circle cx="25%" cy="25%" r="1" fill="white"/><circle cx="35%" cy="35%" r="1.5" fill="white"/><circle cx="45%" cy="28%" r="1" fill="white"/><circle cx="55%" cy="40%" r="1.2" fill="white"/><circle cx="65%" cy="35%" r="1" fill="white"/><circle cx="75%" cy="45%" r="1.5" fill="white"/><circle cx="85%" cy="38%" r="1" fill="white"/><circle cx="95%" cy="50%" r="1.2" fill="white"/></svg>');
      opacity: 0.7;
    }
  }
  
  .sol, .luna {
    position: absolute;
    top: 50%;
    left: 50%;
    border-radius: 50%;
    transform: translate(-50%, -50%);
    transition: box-shadow 0.6s;
    z-index: 2;
  }
  .sol {
    width: 380px;
    height: 380px;
    background: radial-gradient(circle at 60% 40%, #fffbe6 0%, #ffd700 60%, #ff9800 100%);
    box-shadow: 0 0 120px 60px #ffd70088, 0 0 260px 120px #fffbe666;
    z-index: 2;
    animation: efecto-sol 0.6s 3.2s forwards;
  }
  .luna {
    width: 350px;
    height: 350px;
    background: radial-gradient(circle at 40% 60%, #e0e7ef 0%, #b1bedc 100%);
    box-shadow: 0 0 80px 20px #b1bedc77;
    z-index: 3;
    left: calc(50% + 400px);
    animation: mover-luna 1.6s 2s cubic-bezier(.77,0,.18,1) forwards, efecto-luna 0.6s 3.2s forwards;
  }
  .corona {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 600px;
    height: 600px;
    pointer-events: none;
    transform: translate(-50%, -50%);
    border-radius: 50%;
    background: radial-gradient(circle, rgba(255,255,255,0.7) 20%, rgba(0,26,58,0.1) 70%, transparent 100%);
    opacity: 0;
    z-index: 4;
    animation: mostrar-corona 0.6s 3.2s forwards, corona-pulse 0.7s 3.2s both;
  }
  .explosion {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 380px;
    height: 380px;
    border-radius: 50%;
    pointer-events: none;
    background: radial-gradient(circle, #fff 0%, #fff8 40%, transparent 80%);
    opacity: 0;
    transform: translate(-50%, -50%) scale(0.8);
    z-index: 5;
    animation: explosion-anim 1.2s 3.2s forwards;
  }
  .explosion::before,
  .explosion::after {
    content: "";
    position: absolute;
    border-radius: 50%;
    pointer-events: none;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 380px;
    height: 380px;
    background: radial-gradient(circle, rgba(255,255,255,0.4) 0%, transparent 80%);
    opacity: 0.6;
    filter: blur(8px);
    z-index: 6;
  }
  .explosion::after {
    width: 600px;
    height: 600px;
    opacity: 0.3;
    filter: blur(16px);
  }
  .fondo-final {
    position: fixed;
    inset: 0;
    width: 100vw;
    height: 100vh;
    opacity: 0;
    background: url('https://pplx-res.cloudinary.com/image/private/user_uploads/10880907/wifmrOBLDIGsIKj/natalia-volkova-1.jpg') center/cover no-repeat;
    z-index: 10;
    pointer-events: none;
    animation: mostrar-fondo 1.2s 4.3s forwards;
  }
  
  @keyframes mover-luna {
    to {
      left: 50%;
    }
  }
  
  @keyframes mostrar-corona {
    to {
      opacity: 1;
    }
  }
  
  @keyframes mostrar-fondo {
    to {
      opacity: 1;
    }
  }
  
  @keyframes efecto-sol, 
  @keyframes efecto-luna {
    to {
      filter: blur(2px) brightness(1.2);
    }
  }
  
  @keyframes explosion-anim {
    0% {
      opacity: 1;
      transform: translate(-50%, -50%) scale(1);
      box-shadow: 0 0 0 0 #fff, 0 0 0 0 #fff8;
      filter: blur(0px);
    }
    40% {
      opacity: 1;
      transform: translate(-50%, -50%) scale(2.5);
      box-shadow: 0 0 120px 60px #fff, 0 0 300px 120px #fff8;
      filter: blur(12px) brightness(2);
    }
    70% {
      opacity: 1;
      transform: translate(-50%, -50%) scale(6);
      box-shadow: 0 0 180px 100px #fff, 0 0 400px 160px #fff8;
      filter: blur(24px) brightness(2.5);
    }
    100% {
      opacity: 0;
      transform: translate(-50%, -50%) scale(12);
      box-shadow: 0 0 240px 140px #fff, 0 0 600px 220px #fff8;
      filter: blur(40px) brightness(3);
    }
  }
  @keyframes corona-pulse {
    0% { filter: blur(0px) brightness(1); }
    60% { filter: blur(18px) brightness(2.5); }
    100% { filter: blur(10px) brightness(1.2); }
  }
</style>
</head>
<body>
<div class="escena">
  <div class="sol"></div>
  <div class="luna"></div>
  <div class="corona"></div>
  <div class="explosion"></div>
  <div class="fondo-final"></div>
</div>
</body>
</html>
