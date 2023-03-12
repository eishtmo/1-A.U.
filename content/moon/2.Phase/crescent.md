---
title: "Crescent"
date: 2023-03-12T12:48:11-04:00
draft: false
---


<canvas id="crescent" width="500" height="500" style="border:1px dotted white; padding:0px; display:inline;"></canvas>  
<script>
    var c = document.getElementById("crescent");
    var ctx = c.getContext("2d");
    var width = c.width;
    var height = c.height;
 //
    let deg2rad = Math.PI / 180;
// =========================================================			
    function phaseRadius (beta) {
                return Math.sqrt(1 + 0.25 * Math.pow( Math.sin(beta*deg2rad) * Math.tan(beta*deg2rad), 2) );
    }
    function phaseCenterX (beta) {
                return -0.5 * Math.sin(beta*deg2rad) * Math.tan(beta*deg2rad);
    }
    function phaseArclimit (beta) {
                return Math.asin(1 / phaseRadius(beta) );
    }
    //
    function drawCrescent (ctx, Xcrescent, Ycrescent, moonangle) {
            var size = 100;     // Moon size
            var offset;
            var color;
            var moonangleRad = - moonangle * deg2rad;       // Canvas is + CW
        //
            ctx.lineWidth = 1;
            ctx.setLineDash([2,2]);     // dash, gap
        //           
            if ( moonangle >= 0 && moonangle < 90 ) { color = 0;		offset = 0; } 
            if ( moonangle == 90 ) { color = 0;		offset = 0;		moonangle = 90.1; }  
            if ( moonangle > 90 && moonangle < 180 ) { color = 0;		offset = Math.PI; }
            if ( moonangle >= 180 && moonangle < 270 ) { color = 1;		offset = 0;		moonangle = moonangle - 180; }
            if ( moonangle == 270 ) { color = 1;		offset = Math.PI;		moonangle = 90.1; }    
            if ( moonangle > 270 && moonangle <= 360 ) { color = 1; 	offset = Math.PI;	moonangle = moonangle - 180;  }
        //
         // crescent and gibbous half-area					
            ctx.beginPath();
                    if (color == 0 ) { 
            ctx.fillStyle = 'hsl(55, 100%, 50%)'; }  // 55 yellow, 155 lime
                else { 
            ctx.fillStyle = 'hsl(135, 100%, 15%)'; }    // 135 blue
            ctx.arc( Xcrescent, Ycrescent, size, Math.PI/2, -Math.PI/2, true);         
            ctx.arc( Xcrescent + size*phaseCenterX(moonangle), Ycrescent, size*phaseRadius(moonangle), offset - phaseArclimit(moonangle), offset + phaseArclimit(moonangle), false ); 
            ctx.fill();
            // external circle	
            ctx.beginPath();
            ctx.strokeStyle = 'hsl(55, 100%, 40%)';
            ctx.arc( Xcrescent + size*phaseCenterX(moonangle), Ycrescent, size*phaseRadius(moonangle), offset - Math.PI, offset + Math.PI, false ); 
            ctx.stroke();	
        // crescent and gibbous other half-area
            ctx.beginPath();
                if (color == 0 ) { 
            ctx.fillStyle = 'hsl(135, 100%, 15%)'; }
                else { 
            ctx.fillStyle = 'hsl(55, 100%, 50%)'; }
            ctx.arc( Xcrescent, Ycrescent, size, Math.PI/2, -Math.PI/2, false);    
            ctx.arc( Xcrescent + size*phaseCenterX(moonangle), Ycrescent, size*phaseRadius(moonangle),    offset + phaseArclimit(moonangle),  offset - phaseArclimit(moonangle), true );
            ctx.fill();	
            // external circle
            ctx.beginPath();
            ctx.arc( Xcrescent + size*phaseCenterX(moonangle), Ycrescent, size*phaseRadius(moonangle), offset - Math.PI, offset + Math.PI, false ); 
            ctx.stroke();	
        // // moon angle arrow
        //     ctx.beginPath();
        //     ctx.strokeStyle = 'rgb(128,128,128)';
        //     ctx.lineWidth = 1;
        //     ctx.setLineDash([2, 2]);
        //     ctx.arc( Xcrescent, Ycrescent, phaseRadius(moonangle), 0, moonangleRad, true); 
        //     ctx.stroke();
        // // moon angle arrow Arrowheads	
        //     ctx.beginPath();
        //     ctx.save();
        //     var slope = moonangleRad;   // atan2(y,x),  radians
        //     ctx.translate(Xcrescent,Ycrescent);  // sets (x2,y2) to (0, 0)
        //     ctx.rotate(slope);
        //     ctx.lineWidth = 1;
        //     ctx.moveTo(0, 0);
        //     ctx.lineTo(-7, 15);								
        //     ctx.moveTo(0, 0);
        //     ctx.lineTo(5, 15);
        //     ctx.stroke();	
        //     ctx.restore();    
        //     ctx.setLineDash([]);
        // // crescent title
        //     ctx.beginPath();
        //     ctx.save();
        //     ctx.translate(Xcrescent, Ycrescent);
        //     ctx.font = '10px italic Verdana';
        //     ctx.textAlign = 'center';
        //     ctx.textBaseline = 'middle';
        //     ctx.fillStyle = 'rgb(128,128,128)';
        //     ctx.fillText('as seen from Earth', 0, 1.5 * size);
        //     ctx.fill();
        //     ctx.restore();		
    }
// END function drawCrescent (ctx, Xearth, Yearth, moonangle)
//
// drawCrescent(ctx, width/2, height/2, 335);
//
    let thetamin = 40;
    let thetamax = 140;
    let n = 20;
for (var i = 0; i <= n; i++) {
    let angle = thetamin + (i/(n) * (thetamax-thetamin));
    drawCrescent(ctx, width/2, height/2, angle );
}

</script>


