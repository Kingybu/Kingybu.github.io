<!DOCTYPE html>
<html>
<head>
  <title></title>
  <style>
  html, body {
    height: 100%;
    margin: 0;
  }
  body {
    background: black;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  </style>
</head>
<body>
<canvas width="565" height="500" id="game"></canvas>
<script>
const canvas = document.getElementById('game');
const context = canvas.getContext('2d');
const grid = 15;
const playerHeight = grid * 3; // 45
const maxPlayerY = canvas.height - grid - playerHeight;

var playerSpeed = 4;

const leftPlayer = {
  // start in the middle of the game on the left side
  x: grid * 2,
  y: canvas.height / 2 - playerHeight / 2,
  width: grid,
  height: playerHeight,
  
  // shooting cooldown
  cooldown: 0,
  
  // player velocity
  dy: 0
};

const rightPlayer = {
  // start in the middle of the game on the right side
  x: canvas.width - grid * 3,
  y: canvas.height / 2 - playerHeight / 2,
  width: grid,
  height: playerHeight,
  
  // shooting cooldown
  cooldown: 0,
  
  // player velocity
  dy: 0
};

const bullets = {
  speed: 5,
  array: []
}

// check for collision between two objects using axis-aligned bounding box (AABB)
// @see https://developer.mozilla.org/en-US/docs/Games/Techniques/2D_collision_detection
function collides(obj1, obj2) {
  return obj1.x < obj2.x + obj2.width &&
         obj1.x + obj1.width > obj2.x &&
         obj1.y < obj2.y + obj2.height &&
         obj1.y + obj1.height > obj2.y;
}

// game loop
function loop() {
  requestAnimationFrame(loop);
  context.clearRect(0,0,canvas.width,canvas.height);
  
  // bullet cooldowns
  // left player
  if (leftPlayer.cooldown > 0) {
    leftPlayer.cooldown--;
  }
  // right player
  if (rightPlayer.cooldown > 0) {
    rightPlayer.cooldown--;
  }
  
  // move players by their velocity
  leftPlayer.y += leftPlayer.dy;
  rightPlayer.y += rightPlayer.dy;
  
  // prevent players from going through walls
  if (leftPlayer.y < grid) {
    leftPlayer.y = grid;
  }
  else if (leftPlayer.y > maxPlayerY) {
    leftPlayer.y = maxPlayerY;
  }
  
  if (rightPlayer.y < grid) {
    rightPlayer.y = grid;
  }
  else if (rightPlayer.y > maxPlayerY) {
    rightPlayer.y = maxPlayerY;
  }
  
  // draw bullets
  context.fillStyle = 'yellow';
  bullets.array.forEach(function(bullet, index) {
    context.fillRect(bullet.x, bullet.y, 10, 5);
	
	// check if the bullet hits a player
	// left player
	if (collides(bullet, leftPlayer)) {
	  bullets.array.splice(index, 1);
	  leftPlayer.y = canvas.height / 2 - playerHeight / 2;
	  rightPlayer.y = canvas.height / 2 - playerHeight / 2;
	  bullets.array.length = 0;
	}
	// right player
	else if (collides(bullet, rightPlayer)) {
	  bullets.array.splice(index, 1);
	  leftPlayer.y = canvas.height / 2 - playerHeight / 2;
	  rightPlayer.y = canvas.height / 2 - playerHeight / 2;
	  bullets.array.length = 0;
	}
    
    // move bullets
    bullet.x += bullet.dx;
	
	// remove bullets that leave the screen
    if (bullet.x < 0 || bullet.x > canvas.width) {
      bullets.array.splice(index, 1);
    }
  });
  
  // draw paddles
  context.fillStyle = 'gold';
  context.fillRect(leftPlayer.x, leftPlayer.y, leftPlayer.width, leftPlayer.height);
  context.fillRect(rightPlayer.x, rightPlayer.y, rightPlayer.width, rightPlayer.height);

  // draw walls
  context.fillStyle = 'lightgray';
  context.fillRect(0, 0, canvas.width, grid);
  context.fillRect(0, canvas.height - grid, canvas.width, canvas.height);
}

// listen to keyboard events to move the players
document.addEventListener('keydown', function(e) {
  // up arrow key
  if (e.which === 38) {
    rightPlayer.dy = -playerSpeed;
  }
  // down arrow key
  else if (e.which === 40) {
    rightPlayer.dy = playerSpeed;
  }
  
  // w key
  if (e.which === 87) {
    leftPlayer.dy = -playerSpeed;
  }
  // a key
  else if (e.which === 83) {
    leftPlayer.dy = playerSpeed;
  }
  
  // shooting
  // left arrow key
  if (e.which === 37 && rightPlayer.cooldown === 0) {
    bullets.array.push({
	  x: rightPlayer.x - 10, 
	  y: rightPlayer.y + 20, 
	  width: 10,
	  height: 5,
	  dx: -bullets.speed
	});
	rightPlayer.cooldown = 25;
  }
  // d key
  if (e.which === 68 && leftPlayer.cooldown === 0) {
    bullets.array.push({
	  x: leftPlayer.x + 15, 
	  y: leftPlayer.y + 20, 
	  width: 10,
	  height: 5,
	  dx: bullets.speed
	});
	leftPlayer.cooldown = 25;
  }
});
// listen to keyboard events to stop the player if key is released
document.addEventListener('keyup', function(e) {
  if (e.which === 38 || e.which === 40) {
    rightPlayer.dy = 0;
  }
  
  if (e.which === 83 || e.which === 87) {
    leftPlayer.dy = 0;
  }
});
// start the game
requestAnimationFrame(loop);
</script>
</body>
</html>
