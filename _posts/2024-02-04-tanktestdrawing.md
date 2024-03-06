---
toc: true
comments: false
layout: tak
title: tank test drawing
description: A pretty advanced use of JS, HTML, CSS, and Spritesheets to create a single-player game. 
type: hacks
courses: { compsci: {week: 6} }
---

<style>
    body {
        background-color: #4B4B4B;
    }
</style>

<canvas width="1472" height="828" style="border: 4px solid black; float:left; margin:5px; background: #CDB593;" id="box"></canvas>

<script>
    window.addEventListener("keydown", function(e) { if(["Space","ArrowUp","ArrowDown","ArrowLeft","ArrowRight"].indexOf(e.code) > -1) { e.preventDefault(); } }, false);

    var canvas = document.getElementById("box").getContext("2d");
    document.addEventListener ("keydown", keyDownHandler, false);
    document.addEventListener ("keyup", keyUpHandler, false);

    let tank1 = new Image();
    tank1.src = "{{site.baseurl}}/images/sprite/tank1.png";

    let tank2 = new Image () ;
    tank2.src = "{{site.baseurl}}/images/sprite/tank0.png";

    let upPressed = false;
    let downPressed = false;
    let leftPressed = false;
    let rightPressed = false;
    let zeroPressed = false;
    let wPressed = false;
    let sPressed = false;
    let dPressed = false;
    let aPressed = false;
    let spacePressed = false;

    //controls for player one and two
    function keyDownHandler(e)
    {
        if (e.keyCode == 38)
        {
            upPressed = true;
        }

        if(e.keyCode == 40)
        {
            downPressed = true;
        }

        if(e. keyCode == 37)
        {
            leftPressed = true;
        }

        if(e.keyCode == 39)
        {
            rightPressed = true;
        }

        if(e.keyCode == 32)
        {
            spacePressed = true;
        }

        if(e.keyCode == 87)
        {
            wPressed = true;
        }

        if(e.keyCode == 65)
        {
            aPressed = true;
        }

        if(e.keyCode == 83)
        {
            sPressed = true;
        }

        if(e.keyCode == 68)
        {
            dPressed = true;
        }

        if(e.keyCode == 96)
        {
            zeroPressed = true;
        }
    }

    function keyUpHandler(e)
    {
        if (e.keyCode == 38)
        {
            upPressed = false;
        }

        if(e.keyCode == 40)
        {
            downPressed = false;
        }

        if(e. keyCode == 37)
        {
            leftPressed = false;
        }

        if(e.keyCode == 39)
        {
            rightPressed = false;
        }

        if(e.keyCode == 32)
        {
            spacePressed = false;
        }

        if(e.keyCode == 87)
        {
            wPressed = false;
        }

        if(e.keyCode == 65)
        {
            aPressed = false;
        }

        if(e.keyCode == 83)
        {
            sPressed = false;
        }

        if(e.keyCode == 68)
        {
            dPressed = false;
        }

        if(e.keyCode == 96)
        {
            zeroPressed = false;
        }
    }

    function drawimgrotation(img, x, y, width, height, deg)
    {
        let rad = deg * Math.PI / 180;
        canvas.translate( x + width / 2, y + height / 2 );
        canvas.rotate(rad);
        canvas.drawImage( img, width / 2 * -1, height / 2 * -1, width, height);
        canvas.rotate(rad * -1);
        canvas.translate( (x + width / 2) * -1, (y + height / 2) * -1 );
    }

    function controls()
    {

        //p1
        if(leftPressed)
        {
            player1.rotation -= 1;
        }

        if(rightPressed)
        {
            player1.rotation += 1;
        }

        //diag path
        if(upPressed)
        {
            player1.x += Math.cos(player1.rotation * Math.PI/180);
            player1.y += Math.sin(player1.rotation * Math.PI/180);
        }

        if(downPressed)
        {
            player1.x -= Math.cos(player1.rotation * Math.PI/180);
            player1.y -= Math.sin(player1.rotation * Math.PI/180);
        }
        //p2
        if(aPressed)
        {
            player2.rotation -= 1;
        }

        if(dPressed)
        {
            player2.rotation += 1;
        }

        //diag path
        if(wPressed)
        {
            player2.x += Math.cos(player2.rotation * Math.PI/180);
            player2.y += Math.sin(player2.rotation * Math.PI/180);
        }

        if(sPressed)
        {
            player2.x -= Math.cos(player2.rotation * Math.PI/180);
            player2.y -= Math.sin(player2.rotation * Math.PI/180);
        }
    }

    function Player(x, y, rotation, w, h)
    {
        this.x = x;
        this.y = y;
        this.rotation = rotation;
        this.w = w;
        this.h = h;
        this.canfire = true;
        this.hit = false;
        this.score = 0;
    }

    let player2 = new Player (50, 100, 0, 46, 46);
    let player1 = new Player (1300, 650, 180, 46, 46);

    function drawImage()
    {

        controls();

        if(!player1.hit)
        {
            drawimgrotation(tank1, player1.x, player1.y, player1.w, player1.h, player1.rotation);
        }

        if(!player2.hit)
        {
            drawimgrotation(tank2, player2.x, player2.y, player2.w, player2.h, player2.rotation);
        }
    }
    setInterval(drawImage, 10);
</script>