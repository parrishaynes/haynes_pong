<html>

<canvas id="gameCanvas" width="800" height = "600"> </canvas>
 
<script>

var canvas;
var canvasContext;

var ballX =10;
var ballY =100;
var ballSpeedX = 20;
var ballSpeedY = 20;

var Paddle1X = 0;
var Paddle1Y = 0;
var Paddle2X = 0;
var Paddle2Y = 0;
  
var PaddleHeight = 50;
var PaddleWidth = 10;
  
var p1Score = 0;
var p2Score = 0;
  
var winScreen = false
var winScore = 3
  
  
function calculateMousePos(evt){
  var rect = canvas.getBoundingClientRect();
  var root = document.documentElement;
  var mouseX = evt.clientX - rect.left - root.scrollLeft;
  var mouseY = evt.clientY - rect.top - root.scrollTop;
  return { x:mouseX, y:mouseY};
}  

function mouseClick (evt){
  if (winScreen) {
   
   p1Score = 0
   p2Score = 0
    winScreen = false;
  }
}

  
 window.onload = function () {
	
  console.log("hello world");
	canvas = document.getElementById('gameCanvas');
	canvasContext = canvas.getContext('2d');
  
  var fps = 30;
  setInterval ( function (){moveEverything(); drawEverything();}  ,1000/fps);
   
   canvas.addEventListener('mousedown', mouseClick);
   
  canvas.addEventListener('mousemove', function(evt) {
      var mousePos = calculateMousePos(evt);
      Paddle1Y = mousePos.y - PaddleHeight/2;
      //Paddle2Y = mousePos.x - PaddleHeight/2;
  });
    
}

function ballReset (){
  ballX = canvas.width/2;
  ballY = canvas.height/2;
  
  ballSpeedX = -1*ballSpeedX;
  ballSpeedY = -1*ballSpeedY*0.35;
  if(p1Score>=winScore||p2Score>=winScore){
      winScreen = true;
   
    }
  
}
  
function compMove (){
  var paddleSpeed =10
  var paddleCenter = Paddle2Y+PaddleHeight/2
 if (ballY>paddleCenter-35){
 Paddle2Y = Paddle2Y+paddleSpeed;
 }
 if (ballY<paddleCenter+35){
 Paddle2Y = Paddle2Y-paddleSpeed}
}
  
function moveEverything (){
  
  if (winScreen){
    return
   }
    

  ballX = ballX + ballSpeedX;
  ballY = ballY + ballSpeedY;
  
  if (ballX < 0) {
    if (ballY>Paddle1Y && ballY< Paddle1Y+PaddleHeight){
      ballSpeedX = -1 * ballSpeedX;
      var deltaY = ballY - (Paddle1Y+PaddleHeight/2)
      ballSpeedY = deltaY*0.35;
    } else {
      p2Score = p2Score+1;
    ballReset(); 
    console.log(p2Score);
    }
     
  }
  
  
  if (ballX > canvas.width) {
    
   if (ballY>Paddle2Y && ballY< Paddle2Y+PaddleHeight){
      ballSpeedX = -1 * ballSpeedX;
      var deltaY = ballY - (Paddle2Y+PaddleHeight/2)
      ballSpeedY = deltaY*0.35;
  } else {
       p1Score = p1Score+1;
       ballReset(); 
           }
  }
  
  if (ballY<0 || ballY > canvas.height) {
   ballSpeedY = -1 * ballSpeedY;
  }
  
  compMove();
}
  
function colorRect (x, y, w, h, color){
  canvasContext.fillStyle = color;
  canvasContext.fillRect(x,y,w,h);    
  }      

function drawNet (){
  for(var i = 0; i< canvas.height;  i = i +40){
    colorRect (canvas.width/2, i, 2, 20, 'white');
  }

}
  
  function drawEverything (){

  //environmemt
  colorRect(0,0,canvas.width, canvas.height,'grey');
  
    if (winScreen){
      
      if(p1Score>=winScore){
        canvasContext.fillStyle = 'blue';
      canvasContext.fillText('Left Player Won!',canvas.width/2,canvas.height/2);
      } else if (p2Score >= winScore){
           canvasContext.fillStyle = 'red';
      canvasContext.fillText('Right Player Won!',canvas.width/2,canvas.height/2);
      }
      
      canvasContext.fillStyle = 'green';
      canvasContext.fillText('Click To Continue!',canvas.width/2,canvas.height/2 + 200);
    return;
  }
  

  //paddle1
    colorRect(0,Paddle1Y,PaddleWidth,PaddleHeight,'blue');
  
    //paddle2
    colorRect(canvas.width-PaddleWidth,Paddle2Y,PaddleWidth,PaddleHeight,'red');

   // Net
    drawNet();
    
  //ball
    
    canvasContext.beginPath();
    canvasContext.arc(ballX, ballY, 10, 0, Math.PI*2, true);
    canvasContext.fill();
  
  //player1 score
    canvasContext.fillText(p1Score,100,100);
    
  //player2 score
    canvasContext.fillText(p2Score, canvas.width-100, 100);

}

</script>



</html>
