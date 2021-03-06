<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Boyles Tube: JavaLabs.org</title>

</head>
<body>

<h2>Boyle&#8217;s Law</h2>
<p>Boyle’s J-tube experiment is an experiment that Boyle did to prove his law. Boyle experimented with pouring mercury into a J-shaped glass tube with one side blocked.
</p>
<div id="myContainer"></div>

<div style="display: none;">
    <script src="javalab.js"></script>
    <script src="p5.js"></script>

    <script>

    var language = 2;
    var maxMole = 20;
    var xMole = [];
    var yMole = [];
    var vMole = [];
    var aMole = [];
    var xMin = 700;
    var xMax = 850;
    var oyMin = [50, 280];
    var yMin = [50, 360];
    var yMax = [210, 440];
    var press = [1, 3];
    var oxGraph = 100;
    var oyGraph = 380;
    var wGraph = 360;
    var hGraph = 360;
    var xValue = [1, 1.090909091, 1.2, 1.333333333, 1.5, 1.714285714, 2, 2.4, 2.5, 3, 3.5, 4, 4.5, 5, 5.5, 6];
    var yValue = [6, 5.5, 5, 4.5, 4, 3.5, 3, 2.5, 2.4, 2, 1.714285714, 1.5, 1.333333333, 1.2, 1.090909091, 1];
    var xGrid = [0, 1, 2, 3, 4, 5, 6];
    var yGrid = [0, 1, 2, 3, 4, 5, 6];


    function setup() {
        frameRate(20);
        pixelDensity(2 * displayDensity());
        var o = select("#myContainer");
        var a = 0.5;
        var w = int((o.width > (window.innerHeight - 120) / a) ? (window.innerHeight - 120) / a : o.width);
        var h = int(w * a);
        //console.log("o="+o+"w="+w);
        myCanvas = createCanvas(w, h);
        myCanvas.parent("myContainer");
        myCanvas.id("myP5Canvas");
        setup2();
        for (var n = 0; n < 2; n++) {
            xMole[n] = [];
            yMole[n] = [];
            vMole[n] = [];
            aMole[n] = [];
            for (var i = 0; i < maxMole; i++) {
                xMole[n][i] = random(xMin + 10, xMax - 10);
                yMole[n][i] = random(yMin[n] + 10, yMax[n] - 10);
                aMole[n][i] = round(random(360));
                vMole[n][i] = 10
            }
        }
    }

    function windowResized() {
        var o = select("#myContainer");
        var a = 0.5;
        var w = int((o.width > (window.innerHeight - 120) / a) ? (window.innerHeight - 120) / a : o.width);
        var h = int(w * a);
       // console.log("o="+o+"w="+w);
        resizeCanvas(w, h)
    }

    function setup2() {
        for (var n = 0; n < 2; n++) {
            oyMin[n] = yMax[n] - 160 / press[n]
        }
    }

    function draw() {
        if (touches.length > 1) return;
        background(233);
        strokeWeight(1);
        textSize(20);
        textAlign(CENTER, CENTER);
        push();
        scale(width / 900);
        if (language == 0) drawGraph(oxGraph, oyGraph, wGraph, hGraph, 0, 0, 6, 6, xValue, yValue, xGrid, yGrid, "압력 (기압)", "부피 (L)");
        if (language == 1) drawGraph(oxGraph, oyGraph, wGraph, hGraph, 0, 0, 6, 6, xValue, yValue, xGrid, yGrid, "圧力 (気圧)", "体積 (L)");
        if (language == 2) drawGraph(oxGraph, oyGraph, wGraph, hGraph, 0, 0, 6, 6, xValue, yValue, xGrid, yGrid, "Pressure (atm)", "Volume (L)");
        noStroke();
        for (var n = 0; n < 2; n++) {
            var x = lerp(oxGraph, oxGraph + wGraph, press[n] / 6.0);
            var y = lerp(oyGraph, oyGraph - hGraph, 1.0 / press[n]);
            ellipse(x, y, 10, 10);
            text((n == 0) ? "A" : "B", x + 16, y - 16)
        }
        running();
        fill(255);
        noStroke();
        for (var n = 0; n < 2; n++) {
            rect(xMin, yMin[n], xMax - xMin, yMax[n] - yMin[n])
        }
        fill(0, 0, 255);
        stroke(0);
        for (var n = 0; n < 2; n++)
            for (var i = 0; i < maxMole; i++) {
                ellipse(xMole[n][i], yMole[n][i], 5, 5)
            }
        noStroke();
        for (var n = 0; n < 2; n++) {
            fill(128);
            rect(xMin - 10, yMax[n] - 190, 10, 200, 4, 4, 4, 4);
            rect(xMin - 10, yMax[n], xMax - xMin + 20, 10, 4, 4, 4, 4);
            rect(xMax, yMax[n] - 190, 10, 200, 4, 4, 4, 4);
            fill(255, 0, 0);
            rect(xMin, yMin[n] - 10, xMax - xMin, 10);
            fill(0);
            text((n == 0) ? "A" : "B", xMin - 30, yMax[n] - 100)
        }
        fill(0);
        for (var n = 0; n < 2; n++) {
            if (abs(press[n] - 1) < 0.1) {
                drawArrow2D_thick((xMin + xMax) / 2, yMin[n] - 45, 0, 30, 5)
            } else {
                if (abs(int(press[n]) - press[n]) < 0.1) {
                    for (var i = 0; i < int(press[n]); i++) {
                        drawArrow2D_thick(map(i, 0, int(press[n] - 1), xMin + 20, xMax - 20), yMin[n] - 45, 0, 30, 5)
                    }
                } else {
                    for (var i = 0; i < int(press[n]); i++) {
                        drawArrow2D_thick(map(i, 0, int(press[n]), xMin + 20, xMax - 20), yMin[n] - 45, 0, 30, 5)
                    }
                    var h = map(press[n] - int(press[n]), 0, 1, 0, 30);
                    drawArrow2D_thick(xMax - 20, yMin[n] - 15 - h, 0, h, 5)
                }
            }
        }
        noStroke();
        textAlign(RIGHT, CENTER);
        text("P x V =  " + round(press[0] * 1000) / 1000 + " x " + round(6000 / press[0]) / 1000, xMin - 30, yMax[0] - 50);
        text(round(press[1] * 1000) / 1000 + " x " + round(6000 / press[1]) / 1000, xMin - 30, yMax[1] - 50);
        pop();
        drawButtonDrag()
    }

    function drawGraph(x, y, cx, cy, minX, minY, maxX, maxY, valueX, valueY, gridX, gridY, xCaption, yCaption) {
        noStroke();
        fill(255);
        rect(x, y - cy, cx, cy);
        textAlign(CENTER, TOP);
        for (i = 0; i < gridX.length; i++) {
            stroke(0, 192, 0);
            line(lerp(x, x + cx, (gridX[i] - minX) / (maxX - minX)), y, lerp(x, x + cx, (gridX[i] - minX) / (maxX - minX)), y - cy);
            fill(0);
            noStroke();
            text(round(gridX[i]), lerp(x, x + cx, (gridX[i] - minX) / (maxX - minX)), y + 10)
        }
        textAlign(RIGHT, CENTER);
        for (var i = 0; i < gridY.length; i++) {
            stroke(0, 192, 0);
            line(x, lerp(y, y - cy, (gridY[i] - minY) / (maxY - minY)), x + cx, lerp(y, y - cy, (gridY[i] - minY) / (maxY - minY)));
            fill(0);
            noStroke();
            text(round(gridY[i]), x - 10, lerp(y, y - cy, (gridY[i] - minY) / (maxY - minY)))
        }
        stroke(0);
        if (valueX.length > 1)
            for (var i = 0; i < valueX.length - 1; i++)
                if (valueX[i + 1] != 0 || valueY[i + 1] != 0) {
                    line(lerp(x, x + cx, (valueX[i] - minX) / (maxX - minX)), lerp(y, y - cy, (valueY[i] - minY) / (maxY - minY)), lerp(x, x + cx, (valueX[i + 1] - minX) / (maxX - minX)), lerp(y, y - cy, (valueY[i + 1] - minY) / (maxY - minY)))
                }
        line(x, y, x, y - cy);
        line(x, y, x + cx, y);
        fill(0);
        noStroke();
        textAlign(CENTER, TOP);
        text(xCaption, x + cx / 2, y + 40);
        push();
        translate(x - 50, y - cy / 2);
        rotate(HALF_PI);
        text(yCaption, 0, 0);
        pop()
    }

    function spotGraph(x, y, cx, cy, minX, minY, maxX, maxY, valueX, valueY) {
        fill(0);
        ellipse(lerp(x, x + cx, (valueX - minX) / (maxX - minX)), lerp(y, y - cy, (valueY - minY) / (maxY - minY)), 10, 10)
    }

    function running() {
        for (var n = 0; n < 2; n++)
            yMin[n] = lerp(yMin[n], oyMin[n], 0.05);
        var tollerence = 20;
        for (var n = 0; n < 2; n++)
            for (var i = 0; i < maxMole - 1; i++)
                for (var j = i + 1; j < maxMole; j++) {
                    var pix = xMole[n][i] + vMole[n][i] * dcos[aMole[n][i]];
                    var piy = yMole[n][i] + vMole[n][i] * dsin[aMole[n][i]];
                    var pjx = xMole[n][j] + vMole[n][j] * dcos[aMole[n][j]];
                    var pjy = yMole[n][j] + vMole[n][j] * dsin[aMole[n][j]];
                    if (dist(xMole[n][i], yMole[n][i], xMole[n][j], yMole[n][j]) < tollerence)
                        if (dist(xMole[n][i], yMole[n][i], xMole[n][j], yMole[n][j]) > dist(pix, piy, pjx, pjy)) {
                            var vix = vMole[n][i] * dcos[aMole[n][i]];
                            var viy = vMole[n][i] * dsin[aMole[n][i]];
                            var vjx = vMole[n][j] * dcos[aMole[n][j]];
                            var vjy = vMole[n][j] * dsin[aMole[n][j]];
                            var vcx = (vix + vjx) / 2.;
                            var vcy = (viy + vjy) / 2.;
                            var vcix = vix - vcx;
                            var vciy = viy - vcy;
                            var vcjx = vjx - vcx;
                            var vcjy = vjy - vcy;
                            var vci = dist(0, 0, vcix, vciy);
                            var vcj = dist(0, 0, vcjx, vcjy);
                            var aci = getAngle(vcix, vciy);
                            var acj = getAngle(vcjx, vcjy);
                            var aij = getAngle(xMole[n][j] - xMole[n][i], yMole[n][j] - yMole[n][i]);
                            var aji = getAngle(xMole[n][i] - xMole[n][j], yMole[n][i] - yMole[n][j]);
                            aci = mod(2 * aij + 180 - aci, 360);
                            acj = mod(2 * aji + 180 - acj, 360);
                            vix = vci * dcos[aci];
                            viy = vci * dsin[aci];
                            vjx = vcj * dcos[acj];
                            vjy = vcj * dsin[acj];
                            vix += vcx;
                            viy += vcy;
                            vjx += vcx;
                            vjy += vcy;
                            var t = vMole[n][i] + vMole[n][j];
                            vMole[n][i] = dist(0, 0, vix, viy);
                            vMole[n][j] = t - vMole[n][i];
                            aMole[n][i] = getAngle(vix, viy);
                            aMole[n][j] = getAngle(vjx, vjy)
                        }
                }
        for (var n = 0; n < 2; n++)
            for (var i = 0; i < maxMole; i++) {
                {
                    xMole[n][i] += vMole[n][i] * dcos[aMole[n][i]];
                    yMole[n][i] += vMole[n][i] * dsin[aMole[n][i]]
                }
                if (xMole[n][i] < (xMin + 2)) {
                    xMole[n][i] = 2 * (xMin + 2) - xMole[n][i];
                    aMole[n][i] = mod(180 - aMole[n][i], 360)
                }
                if (xMole[n][i] > (xMax - 2)) {
                    xMole[n][i] = 2 * (xMax - 2) - xMole[n][i];
                    aMole[n][i] = mod(180 - aMole[n][i], 360)
                }
                if (yMole[n][i] < (yMin[n] + 2)) {
                    yMole[n][i] = 2 * (yMin[n] + 2) - yMole[n][i];
                    aMole[n][i] = 360 - aMole[n][i];
                    yMin[n] -= 1.0 / press[n]
                }
                if (yMole[n][i] > (yMax[n] - 2)) {
                    yMole[n][i] = 2 * (yMax[n] - 2) - yMole[n][i];
                    aMole[n][i] = 360 - aMole[n][i]
                }
            }
    }

    var dragged = -1;
    var dragX;
    var offsetX;

    function touchStarted() {
        if (!contain(mouseX, mouseY, 0, 0, width, height)) return;
        var x0 = lerp(oxGraph, oxGraph + wGraph, press[0] / 6.0);
        var x1 = lerp(oxGraph, oxGraph + wGraph, press[1] / 6.0);
        var y0 = lerp(oyGraph, oyGraph - hGraph, 1.0 / press[0]);
        var y1 = lerp(oyGraph, oyGraph - hGraph, 1.0 / press[0]);
        var d0 = dist(mouseX * 900 / width, mouseY * 900 / width, x0, y0);
        var d1 = dist(mouseX * 900 / width, mouseY * 900 / width, x1, y1);
        if (d0 < d1) {
            dragged = 0;
            dragX = x0;
            offsetX = mouseX * 900 / width - dragX
        } else {
            dragged = 1;
            dragX = x1;
            offsetX = mouseX * 900 / width - dragX
        }
    }

    function touchMoved() {
        if (touches.length > 1) return;
        if (dragged >= 0) {
            dragX = mouseX * 900 / width - offsetX;
            press[dragged] = round((dragX - oxGraph) * 12.0 / wGraph) / 2.0;
            press[dragged] = constrain(press[dragged], 1, 6);
            setup2();
            return !1
        }
    }

    function touchEnded() {
        dragged = -1
    }
</script>
</div>

</body>

</html>
