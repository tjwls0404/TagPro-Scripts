// ==UserScript==
// @name       		Nyan Cat
// @version    		1
// @description  	Creates a rainbow trail!
// @include			http://tagpro-*.koalabeast.com:*
// @include			http://tangent.jukejuice.com:*
// @include			http://*.newcompte.fr:*
// @author  		Browncoat (edited by Lysozyme, sound by Dr. Holmes)
// ==/UserScript==

/**
 * Define a few namespaces to prevent false IDE warnings
 * @namespace tagpro
 * @namespace tagpro.tiles
 * @namespace tagpro.viewPort
 * @namespace player.degree
 * @namespace player.dead
 * @namespace player.flag
*/

//  Main tagpro function
tagpro.ready(function () {
    
    if (tagpro.events.drawPlayer)
        return;
//____________________________OPTIONS, CHANGE THIS STUFF AND THINGS HAPPEN__________________________________    
    
    //____━━____┓━╭━━━━━╮
    //____━━____┗┓|:::: ^━━━^
    //____━━____━┗|::::|｡◕‿‿◕｡|
    //____━━____━━╰O--O--O—O 
    
    
    //Change this for when the rainbow is shown coming out of the cat's arse. hue.
    
    rainbowcondition = "flag";
    
    //"all" for always shown.
    //"pups" for only on balls with powerups.
    //"flag" for only on balls holding a flag.
    //"off" for off (or just delete the script!)
    
    
    //Change this for when nyan cat music is played. atm only op 
    
    soundcondition = "flag";
    
    //"flag" to play sound when you grab the flag
    //"off" for off. I can't recommend this option more, especially if you are easily irritated
    
//__________________END OPTIONS, CHANGE BELOW AT RISK OF CREATING INVISIBALLS OR SOME OTHER CRAZY SHIZZ___________________
    
        
    spin_on = "on"
    
    var SpinType = {
        OFF: 0,
        WHOLE_BALL: 1,
        OVERLAY: 2
    };    
    
    if {spin_on == "on"){
        var spinType = SpinType.1;
       }else{
        var spinType = SpinType.0;
    
    
    var showDropShadows = false;
    var showBallShine = false;
    var ballShineUrl = "http://i.imgur.com/YsZccQv.png";
    var centerFlair = false;
    var spinOverlayUrl = "http://i.imgur.com/KtwiVqW.png";
    var particleFactor = 0.5;
    var showParticles = true;
    
    var particles = [];
    var drawDropShadowCallbacks = [];
    var drawPlayerCallbacks = [];
    var drawPlayerInfoCallbacks = [];
    var offset = {
        x: 0,
        y: 0
    };
    var viewBox = {
        left: 0,
        top: 0,
        right: 0,
        bottom: 0,
        leftClip: 0,
        topClip: 0,
        rightClip: 0,
        bottomClip: 0
    };
    
    function addAsset(id, url) {
        var asset = document.createElement("img");
        asset.src = url;
        asset.id = id;
        asset.className = "asset";
        $("#assets").append(asset.outerHTML);
    }
    
    addAsset("shine", ballShineUrl);
    addAsset("spinOverlay", spinOverlayUrl);
    
    //  Our custom draw function.
    var superUiDraw = tagpro.ui.draw;
    tagpro.ui.draw = function(context) {
        updateViewBox(context);
        doAll(drawDropShadowCallbacks);
        drawParticles(context, Layer.UNDER_PLAYERS);
        doAll(drawPlayerCallbacks);
        drawParticles(context, Layer.OVER_PLAYERS);
        doAll(drawPlayerInfoCallbacks);
        superUiDraw(context);
    };
    
    function updateViewBox(context) {
        var e = 20;
        var canvas = $("canvas");
        var a = {
            x: canvas.attr("width")/2,
            y: canvas.attr("height")/2
        };
        viewBox.left = Math.round(tagpro.viewPort.source.x / tagpro.zoom) * tagpro.zoom - a.x * tagpro.zoom + e;
        viewBox.top = Math.round(tagpro.viewPort.source.y / tagpro.zoom) * tagpro.zoom - a.y * tagpro.zoom + e;
        viewBox.right = Math.round(tagpro.viewPort.source.x / tagpro.zoom) * tagpro.zoom + a.x * tagpro.zoom + e;
        viewBox.bottom = Math.round(tagpro.viewPort.source.y / tagpro.zoom) * tagpro.zoom + a.y * tagpro.zoom + e;
        viewBox.leftClip = 0; viewBox.topClip = 0; viewBox.rightClip = 0; viewBox.bottomClip = 0;
        viewBox.left < 0 && (viewBox.leftClip = -viewBox.left);
        viewBox.top < 0 && (viewBox.topClip = -viewBox.top);
        viewBox.right > context.canvas.width && (viewBox.rightClip = viewBox.right - context.canvas.width);
        viewBox.bottom > context.canvas.height && (viewBox.bottomClip = viewBox.bottom - context.canvas.height);
    }
    
    function doAll(callbacks) {
        while (callbacks.length > 0) {
            (callbacks.shift())();
        }
    }
    
    function drawParticles(context, layer) {
        for (var i = 0; i < particles.length; i++) {
            var particle = particles[i];
            particle.draw(context, offset, layer);
            if (particle.shouldBeRemoved()) {
                particles.splice(i, 1);
                i--;
            }
        }
    }
    
    tagpro.events.register({
        drawPlayer: function (player, context, drawPos, TILESIZE) {
            
            if (tagpro.viewPort.source == player) {
                offset.x = drawPos.x - player.x;
                offset.y = drawPos.y - player.y;
            }
            
            drawDropShadowCallbacks.push(function() {
                drawDropShadow(context, drawPos);
            });
            
            drawPlayerCallbacks.push(function () {
                drawPlayer(player, context, drawPos, TILESIZE);
            });
            
            drawPlayerInfoCallbacks.push(function () {
                drawPlayerInfo(context, player);
            });
            
            addParticles(player);
        }
    });
    
    // -----------------------------------------------------------
    // --------------------SOUND SCRIPT---------------------------   
    // -----------------------------------------------------------
    if(soundcondition == "flag"){
        nyanMusic = [
            "http://k007.kiwi6.com/hotlink/mfz86avv7x/nyan.mp3",
            "http://k007.kiwi6.com/hotlink/mfz86avv7x/nyan.mp3",
            "http://k007.kiwi6.com/hotlink/mfz86avv7x/nyan.mp3",
            "http://k007.kiwi6.com/hotlink/mfz86avv7x/nyan.mp3",
            "http://k007.kiwi6.com/hotlink/z3cqmvyocq/retarded_nyan.mp3"
        ]
        
        
        
        // Nyan Music
        tempNyan = nyanMusic[0]
        nyan = document.createElement('audio');
        nyan.setAttribute('id', 'nyan');
        nyan.setAttribute('preload', 'auto');
        nyan.setAttribute('loop', '');
        assets.appendChild(nyan)
        nyan.volume = 0.6
        nyan.innerHTML = '<source src="' + tempNyan + '" type="audio/' + tempNyan.split('.').pop() + '">';
        
        
        
        // Script
        function nyanSound() {
            var flag = tagpro.players[tagpro.playerId].flag
            if (flag) {
                var nyanCat = document.getElementById('nyan');
                var musicth = document.getElementById('music');
                var rand = Math.floor(Math.random() * (5));
                
                if (document.getElementById('soundMusic').getAttribute('class') !== 'off') {
                    musicth.pause();
                    
                };
                
                nyanCat.innerHTML = '<source src="' + nyanMusic[rand] + '" type="audio/' + nyanMusic[rand].split('.').pop() + '">';
                nyanCat.pause();
                nyanCat.load();
                nyanCat.play();
            }
        }
        
        function nyanOff() {
            var nyanCat = document.getElementById('nyan');
            var musicth = document.getElementById('music');
            nyanCat.pause();
            if (document.getElementById('soundMusic').getAttribute('class') !== 'off') {
                musicth.play();
            }
        }
        
        
        
        
        tagpro.socket.on("sound", function(message) {
            sound = message.s
            if (["friendlyalert","placeholder"].indexOf(sound)>-1) {
                setTimeout(function(){nyanSound();}, 30);
                
            } else if (["friendlydrop","placeholder"].indexOf(sound)>-1) {
                nyanOff();
                img.style.visibility = 'hidden';
            } else if (["cheering","placeholder"].indexOf(sound)>-1) {
                nyanOff();
                img.style.visibility = 'hidden';
            }
                });
    }
    // -----------------------------------------------------------
    // -----------------------------------------------------------
    
    
    function addParticles(player) {
        
        var centerX = player.x + 20;
        var centerY = player.y + 20;
        
        if (rainbowcondition == "all"){
            
            
            if (player.team === 1 && player.grip == false) {
                
                addParticle(new Particle(centerX-7, centerY-7, 2.5, "250, 0, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                var radius = Math.floor(random(3, 7));
                
                addParticle(new Particle(centerX-7, centerY-4, 3, "251, 154, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                var radius = Math.floor(random(3, 7));
                
                addParticle(new Particle(centerX-7, centerY-1, 2.5, "253, 255, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                var radius = Math.floor(random(3, 7));
                
                addParticle(new Particle(centerX-7, centerY+1, 2.5, "50, 255, 4", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                var radius = Math.floor(random(3, 7));
                
                addParticle(new Particle(centerX-7, centerY+4, 3, "0, 152, 255", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                var radius = Math.floor(random(3, 7));
                
                addParticle(new Particle(centerX-7, centerY+7, 2.5, "103, 52, 253", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                var radius = Math.floor(random(3, 7));
            }
            
            if (player.team ==2 && player.grip == false) {
                
                addParticle(new Particle(centerX+7, centerY-7, 2.5, "250, 0, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                var radius = Math.floor(random(3, 7));
                
                addParticle(new Particle(centerX+7, centerY-4, 3, "251, 154, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                var radius = Math.floor(random(3, 7));
                
                addParticle(new Particle(centerX+7, centerY-1, 2.5, "253, 255, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                var radius = Math.floor(random(3, 7));
                
                addParticle(new Particle(centerX+7, centerY+1, 2.5, "50, 255, 4", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                var radius = Math.floor(random(3, 7));
                
                addParticle(new Particle(centerX+7, centerY+4, 3, "0, 152, 255", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                var radius = Math.floor(random(3, 7));
                
                addParticle(new Particle(centerX+7, centerY+7, 2.5, "103, 52, 253", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                var radius = Math.floor(random(3, 7));
            }
            
            if (player.grip) {
                addParticle(new Particle(centerX, centerY, 10, "255, 44, 228", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                var radius = Math.floor(random(3, 7));
            }
        } 
        
        if (rainbowcondition == "pups"){
            
            if ((player.tagpro || player.bomb) && player.grip == false){
                
                if (player.team === 1) {
                    
                    addParticle(new Particle(centerX-7, centerY-7, 2.5, "250, 0, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX-7, centerY-4, 3, "251, 154, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX-7, centerY-1, 2.5, "253, 255, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX-7, centerY+1, 2.5, "50, 255, 4", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX-7, centerY+4, 3, "0, 152, 255", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX-7, centerY+7, 2.5, "103, 52, 253", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    
                } else {
                    
                    addParticle(new Particle(centerX+7, centerY-7, 2.5, "250, 0, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX+7, centerY-4, 3, "251, 154, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX+7, centerY-1, 2.5, "253, 255, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX+7, centerY+1, 2.5, "50, 255, 4", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX+7, centerY+4, 3, "0, 152, 255", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX+7, centerY+7, 2.5, "103, 52, 253", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                }
                
            }
            
            if (player.grip){
                addParticle(new Particle(centerX, centerY, 10, "255, 44, 228", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                var radius = Math.floor(random(3, 7));
            }
            
        } 
        
        
        if (rainbowcondition == "flag"){
            
            if (player.flag && player.grip == false){
                
                if (player.team === 1) {
                    
                    addParticle(new Particle(centerX-7, centerY-7, 2.5, "250, 0, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX-7, centerY-4, 3, "251, 154, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX-7, centerY-1, 2.5, "253, 255, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX-7, centerY+1, 2.5, "50, 255, 4", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX-7, centerY+4, 3, "0, 152, 255", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX-7, centerY+7, 2.5, "103, 52, 253", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    
                } else {
                    
                    addParticle(new Particle(centerX+7, centerY-7, 2.5, "250, 0, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX+7, centerY-4, 3, "251, 154, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX+7, centerY-1, 2.5, "253, 255, 0", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX+7, centerY+1, 2.5, "50, 255, 4", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX+7, centerY+4, 3, "0, 152, 255", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                    
                    addParticle(new Particle(centerX+7, centerY+7, 2.5, "103, 52, 253", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                    var radius = Math.floor(random(3, 7));
                }
                
            }
            if (player.grip){
                addParticle(new Particle(centerX, centerY, 10, "255, 44, 228", 500, 0, 0.8, Layer.UNDER_PLAYERS), 1);
                var radius = Math.floor(random(3, 7));
            }
        } 
    } 
    
    
    
    
    
    function addParticle(particle, overrideFactor) {
        if (showParticles) {
            var total = 1;
            if (overrideFactor) {
                total = overrideFactor * total;
            } else {
                total = total * particleFactor;
            }
            for (var i = 0; i < total; i++) {
                particles.push(particle);
            }
        }
    }
    
    function transformPosition(p) {
        return {
            x: Math.round(p.x * (1 / tagpro.zoom)) - Math.round(viewBox.left * (1 / tagpro.zoom)),
            y: Math.round(p.y * (1 / tagpro.zoom)) - Math.round(viewBox.top * (1 / tagpro.zoom))
        };
    }
    
    function drawDropShadow(context, drawPos) {
        if (showDropShadows) {
            context.save();
            context.beginPath();
            context.arc(drawPos.x + 20 * (1 / tagpro.zoom), drawPos.y + 20 * (1 / tagpro.zoom), 18 * (1 / tagpro.zoom), 0, Math.PI * 2, true);
            context.fillStyle = "rgba(0, 0, 0, 1)";
            context.shadowColor = 'rgba(0, 0, 0, 0.5)';
            context.shadowBlur = 10;
            context.shadowOffsetX = -5;
            context.shadowOffsetY = 5;
            context.fill();
            context.closePath();
            context.restore();
        }
    }
    
    function drawPlayer(player, context, drawPos, TILESIZE) {
        context.save();
        
        var rotateCanvas = function () {
            context.translate(drawPos.x + (TILESIZE / 2) * (1 / tagpro.zoom), drawPos.y + (TILESIZE / 2) * (1 / tagpro.zoom));
            context.rotate(player.angle);
            context.translate(-drawPos.x - (TILESIZE / 2) * (1 / tagpro.zoom), -drawPos.y - (TILESIZE / 2) * (1 / tagpro.zoom));
        };
        
        if (spinType == SpinType.WHOLE_BALL) {
            rotateCanvas();
        }
        
        tagpro.tiles.drawWithZoom(context, player.team == 1 ? "redball" : "blueball", drawPos);
        
        // flair in center
        var e = $("#flair").get(0);
        if (centerFlair && player.flair && tagpro.zoom <= 1) {
            context.drawImage(e, player.flair.x * 16, player.flair.y * 16, 16, 16, drawPos.x + 12 * (1 / tagpro.zoom), drawPos.y + 13 * (1 / tagpro.zoom), 16, 16);
        }
        
        if (player.bomb && Math.round(Math.random() * 4) == 1) {
            context.fillStyle = "rgba(255, 255, 0, .50)";
            context.beginPath();
            context.arc(drawPos.x + TILESIZE / 2 * (1 / tagpro.zoom), drawPos.y + TILESIZE / 2 * (1 / tagpro.zoom), 19 * (1 / tagpro.zoom), 0, Math.PI * 2, true);
            context.closePath();
            context.fill();
        }
        
        if (player.tagpro) {
            context.strokeStyle = "#00FF00";
            context.fillStyle = "rgba(0, 255, 0, .25)";
            context.lineWidth = 2 * (1 / tagpro.zoom);
            context.beginPath();
            context.arc(drawPos.x + TILESIZE / 2 * (1 / tagpro.zoom), drawPos.y + TILESIZE / 2 * (1 / tagpro.zoom), 19 * (1 / tagpro.zoom), 0, Math.PI * 2, true);
            context.closePath();
            if (!player.bomb) {
                context.fill();
            }
            context.stroke();
        }
        
        if (spinType == SpinType.OVERLAY) {
            rotateCanvas();
            var spinOverlay = $("#spinOverlay").get(0);
            var p = (player.team == 1) ? 0 : 1;
            context.drawImage(spinOverlay, 40 * p, 0, 40, 40, drawPos.x, drawPos.y, 40 / tagpro.zoom, 40 / tagpro.zoom);
        }
        
        context.restore();
        
        //shine
        if (showBallShine) {
            var s = $("#shine").get(0);
            context.drawImage(s, drawPos.x + 1, drawPos.y + 1, 40 / tagpro.zoom, 40 / tagpro.zoom);
        }
        
        // Flair in default position
        if (!centerFlair && player.flair && tagpro.zoom <= 1) {
            context.drawImage(e, player.flair.x * 16, player.flair.y * 16, 16, 16, drawPos.x + 12, drawPos.y - 17, 16, 16);
        }
        
    }
    
    // Username, flags and degrees
    var flagNames = ["redflag", "blueflag", "yellowflag"];
    function drawPlayerInfo(context, player) {
        var p = transformPosition(player);
        if (!player.dead && player.draw) {
            
            // Flag
            if (player.flag) {
                var flag = "bc_" + flagNames[player.flag - 1];
                tagpro.tiles.drawWithZoom(context, flag, {
                    x: p.x + Math.round(13 * (1 / tagpro.zoom)),
                    y: p.y - Math.round(32 * (1 / tagpro.zoom))
                });
            }
            
            // Degrees
            if (tagpro.zoom <= 1.5 && player.browncoatDegreeCache) {
                context.drawImage(
                    player.browncoatDegreeCache.canvas,
                    p.x + 20 * (1 / tagpro.zoom),
                    p.y - Math.round(8 * (1 / tagpro.zoom))
                );
            }
            
            // Name
            if (player.name && tagpro.zoom <= 4 && player.browncoatCache) {
                context.drawImage(
                    player.browncoatCache.canvas,
                    p.x + Math.round(18 * (1 / tagpro.zoom)),
                    p.y - 20 * (1 / tagpro.zoom)
                );
            }
            
        }
    }
    
    // Add effects on bomb explosion
    onSocketEvent("bomb", function (e) {
        // e = top left corner of tile
        for (var i = 0; i < 3; i++) {
            
            // Black fragments
            for (var j = 0; j < 3; j++) {
                var speed = {
                    x: random(-7, 7),
                    y: random(-7, 7)
                };
                addParticle(new Particle(e.x + 20, e.y + 20, 2, "0, 0, 0", 500, speed));
            }
            
            // Grey smoke
            var radius = Math.random() * 5 + 10;
            var x = e.x + random(0, 40);
            var y = e.y + random(0, 40);
            addParticle(new Particle(x, y, radius, "51, 51, 51", 1000));
            
            // White flash
            x = e.x + random(0, 40);
            y = e.y + random(0, 40);
            addParticle(new Particle(x, y, 10, "255, 255, 255", 200));
        }
    });
    
    // Add effects on player pop
    onSocketEvent("splat", function (player) {
        for (var i = 0; i < 30; i++) {
            var rgb = player.t == 1 ? "255, 0, 0" : "0, 0, 255";
            var speed = {
                x: random(-4, 4),
                y: random(-4, 4)
            };
            var x = player.x + 20 + speed.x * random(0, 20);
            var y = player.y + 20 + speed.y * random(0, 20);
            addParticle(new Particle(x, y, 1, rgb, 300, speed));
        }
    });
    
    function onSocketEvent(event, handler) {
        tagpro.socket.on(event, function(param) {
            try {
                handler(param);
            } catch (err) {
                console.log("Browncoat's mod error handling event '" + event + "': " + err);
            }
        });
    }
    
    
    // -------------------------- OVERRIDES --------------------------
    
    
    // Don't draw the speed/grip icon over players unless particles are disabled
    // Don't draw flags - done in drawPlayerInfo()
    var superDrawWithZoom = tagpro.tiles.drawWithZoom;
    tagpro.tiles.drawWithZoom = function (context, type, position, r, i, s) {
        if ((!showParticles || type != "grip") && type != "yellowflag" && type != "redflag" && type != "blueflag") {
            if (type.substr(0, 3) == "bc_") {
                type = type.substr(3, type.length);
            }
            superDrawWithZoom(context, type, position, r, i, s);
        }
    };
    
    tagpro.flair.draw = function (context, drawPos, flair) {
        // Do nothing - flair is draw in the drawPlayerInfo() function
        //        t.drawImage(e, r.x * 16, r.y * 16, 16, 16, n.x + 12, n.y - 17, 16, 16)
    };
    
    tagpro.ui.createPlayerCache = function(player) {
        player.cache = {
            canvas: $("<canvas></canvas>").attr("width", 140).attr("height", 35).attr("visible", false).get(0)
        };
        player.degreeCache = {
            canvas: $("<canvas></canvas>").attr("width", 45).attr("height", 35).attr("visible", false).get(0)
        };
        player.cache.context = player.cache.canvas.getContext("2d");
        player.degreeCache.context = player.degreeCache.canvas.getContext("2d");
        
        player.browncoatCache = {
            canvas: $("<canvas></canvas>").attr("width", 140).attr("height", 35).get(0)
        };
        player.browncoatDegreeCache = {
            canvas: $("<canvas></canvas>").attr("width", 45).attr("height", 35).get(0)
        };
        player.browncoatCache.context = player.browncoatCache.canvas.getContext("2d");
        player.browncoatDegreeCache.context = player.browncoatDegreeCache.canvas.getContext("2d");
        player.cache.update = function () {
            // Hide default caches and create new caches to draw on to in drawPlayerInfo()
            // Couldn't figure out how to override default draw name/flair so we hide default and create new canvases
            player.browncoatCache.context.clearRect(0, 0, 140, 35);
            player.browncoatDegreeCache.context.clearRect(0, 0, 45, 35);
            var nameColor = player.auth ? "#BFFF00" : "#ffffff";
            if (tagpro.settings.ui.names) {
                tagpro.prettyText(player.name, 15, 15, nameColor, false, false, player.browncoatCache.context);
            }
            if (player.degree && tagpro.settings.ui.degrees) {
                tagpro.prettyText(player.degree + "°", 20, 15, "#ffffff", false, false, player.browncoatDegreeCache.context);
            }
        };
        player.cache.update();
    };
    
    function random(min, max) {
        return Math.random() * (max - min) + min;
    }
    
    var Layer = {
        UNDER_PLAYERS: 1,
        OVER_PLAYERS: 2
    };
    
    /**
     *
     * Particle class
     *
     * @constructor
     * @param x
     * @param y
     * @param radius
     * @param {String} rgb should be in the form "255, 255, 255"
     * @param lifeTime
     * @param [speed]
     * @param [alpha]
     * @param {int} [layer]
     */
    var Particle = function (x, y, radius, rgb, lifeTime, speed, alpha, layer) {
        speed = speed || {x: 0, y: 0};
        alpha = alpha || 1;
        layer = layer || Layer.OVER_PLAYERS;
        this.x = x;
        this.y = y;
        this.radius = radius;
        this.rgb = rgb;
        this.alpha = alpha;
        this.initialAlpha = alpha;
        this.remove = false;
        this.lifeTime = lifeTime;
        this.timer = 0;
        this.speed = speed;
        this.layer = layer;
    };
    
    Particle.prototype.draw = function (context, offset, layer) {
        if (!this.remove && this.layer == layer) {
            this.x += this.speed.x;
            this.y += this.speed.y;
            context.save();
            context.fillStyle = "rgba(" + this.rgb + ", " + this.alpha + ")";
            context.beginPath();
            context.arc(this.x + offset.x, this.y + offset.y, this.radius * (1 / tagpro.zoom), 0, Math.PI * 2, true);
            context.closePath();
            context.fill();
            context.restore();
            
            this.timer += 1000 / 60;
            this.alpha = (1 - (this.timer / this.lifeTime)) * this.initialAlpha;
            if (this.alpha < 0.001 || this.timer >= this.lifeTime) {
                this.remove = true;
            }
        }
    };
    
    Particle.prototype.shouldBeRemoved = function () {
        return this.remove;
    };
    
});
