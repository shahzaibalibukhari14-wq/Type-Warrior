<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TYPE WARRIOR — Battle with Words</title>
<link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700;900&family=Rajdhani:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
*,*::before,*::after{margin:0;padding:0;box-sizing:border-box;}
:root{
  --bg:#0a0a0f;
  --bg2:#0f0f1a;
  --surface:#13131f;
  --surface2:#1a1a2e;
  --gold:#f0b429;
  --gold2:#ffd700;
  --red:#e53e3e;
  --red2:#fc8181;
  --blue:#4299e1;
  --blue2:#90cdf4;
  --green:#48bb78;
  --purple:#9f7aea;
  --cyan:#0bc5ea;
  --orange:#ed8936;
  --white:#f7fafc;
  --muted:#718096;
  --border:rgba(240,180,41,0.15);
}

html,body{height:100%;background:var(--bg);}
body{
  font-family:'Rajdhani',sans-serif;
  color:var(--white);
  overflow:hidden;
  cursor:crosshair;
}

/* ===== BACKGROUND ===== */
.bg-layer{
  position:fixed;inset:0;z-index:0;
  background:
    radial-gradient(ellipse 80% 50% at 50% 0%, rgba(159,122,234,0.06) 0%, transparent 60%),
    radial-gradient(ellipse 60% 40% at 0% 100%, rgba(229,62,62,0.05) 0%, transparent 60%),
    radial-gradient(ellipse 60% 40% at 100% 100%, rgba(66,153,225,0.05) 0%, transparent 60%),
    linear-gradient(180deg,#0a0a0f 0%,#0f0f1a 100%);
}

/* Animated scanlines */
.bg-layer::after{
  content:'';position:absolute;inset:0;
  background:repeating-linear-gradient(0deg,transparent,transparent 3px,rgba(0,0,0,0.03) 3px,rgba(0,0,0,0.03) 4px);
  pointer-events:none;
}

/* Grid lines */
.bg-grid{
  position:fixed;inset:0;z-index:0;
  background-image:
    linear-gradient(rgba(240,180,41,0.03) 1px, transparent 1px),
    linear-gradient(90deg,rgba(240,180,41,0.03) 1px,transparent 1px);
  background-size:60px 60px;
}

/* Particle canvas */
#particleCanvas{position:fixed;inset:0;z-index:1;pointer-events:none;}

/* ===== MAIN LAYOUT ===== */
#app{position:relative;z-index:10;width:100vw;height:100vh;display:flex;flex-direction:column;}

/* ===== TITLE SCREEN ===== */
#titleScreen{
  position:absolute;inset:0;z-index:50;
  display:flex;flex-direction:column;align-items:center;justify-content:center;
  background:linear-gradient(180deg,rgba(10,10,15,0.97) 0%,rgba(15,15,26,0.97) 100%);
  gap:2rem;
}

.title-glow{
  font-family:'Cinzel',serif;
  font-size:clamp(3rem,8vw,6rem);
  font-weight:900;
  letter-spacing:0.1em;
  text-align:center;
  background:linear-gradient(135deg,var(--gold),var(--gold2),var(--orange));
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;
  filter:drop-shadow(0 0 40px rgba(240,180,41,0.4));
  animation:titlePulse 3s ease-in-out infinite;
  line-height:1.1;
}
@keyframes titlePulse{
  0%,100%{filter:drop-shadow(0 0 30px rgba(240,180,41,0.4));}
  50%{filter:drop-shadow(0 0 60px rgba(240,180,41,0.7));}
}

.title-sub{
  font-size:1.1rem;letter-spacing:0.5em;
  text-transform:uppercase;color:var(--muted);
  text-align:center;
}

.title-divider{
  width:200px;height:1px;
  background:linear-gradient(90deg,transparent,var(--gold),transparent);
}

.difficulty-select{
  display:flex;gap:1rem;flex-wrap:wrap;justify-content:center;
}
.diff-btn{
  padding:0.7rem 1.8rem;
  font-family:'Rajdhani',sans-serif;font-size:1rem;font-weight:700;
  letter-spacing:0.15em;text-transform:uppercase;
  border:1px solid;border-radius:3px;cursor:pointer;
  transition:all 0.2s;background:none;
}
.diff-btn.easy{color:var(--green);border-color:var(--green);}
.diff-btn.easy:hover,.diff-btn.easy.selected{background:rgba(72,187,120,0.15);box-shadow:0 0 20px rgba(72,187,120,0.3);}
.diff-btn.medium{color:var(--gold);border-color:var(--gold);}
.diff-btn.medium:hover,.diff-btn.medium.selected{background:rgba(240,180,41,0.15);box-shadow:0 0 20px rgba(240,180,41,0.3);}
.diff-btn.hard{color:var(--red);border-color:var(--red);}
.diff-btn.hard:hover,.diff-btn.hard.selected{background:rgba(229,62,62,0.15);box-shadow:0 0 20px rgba(229,62,62,0.3);}
.diff-btn.insane{color:var(--purple);border-color:var(--purple);}
.diff-btn.insane:hover,.diff-btn.insane.selected{background:rgba(159,122,234,0.15);box-shadow:0 0 20px rgba(159,122,234,0.3);}

.start-btn{
  padding:1rem 3rem;
  font-family:'Cinzel',serif;font-size:1.2rem;font-weight:700;
  letter-spacing:0.2em;text-transform:uppercase;
  background:linear-gradient(135deg,#b7791f,var(--gold),#b7791f);
  color:#000;border:none;border-radius:3px;cursor:pointer;
  box-shadow:0 0 30px rgba(240,180,41,0.4),inset 0 1px 0 rgba(255,255,255,0.2);
  transition:all 0.2s;
  animation:glowBtn 2s ease-in-out infinite;
}
@keyframes glowBtn{0%,100%{box-shadow:0 0 20px rgba(240,180,41,0.4);}50%{box-shadow:0 0 40px rgba(240,180,41,0.8);}}
.start-btn:hover{transform:scale(1.04);}

.title-instructions{
  text-align:center;color:var(--muted);
  font-size:0.85rem;line-height:1.8;
  max-width:500px;letter-spacing:0.05em;
}
.title-instructions strong{color:var(--gold);}

/* ===== GAME OVER SCREEN ===== */
#gameOver{
  position:absolute;inset:0;z-index:50;
  display:none;flex-direction:column;
  align-items:center;justify-content:center;
  background:rgba(10,10,15,0.95);backdrop-filter:blur(10px);
  gap:1.5rem;
}
#gameOver.show{display:flex;}

.go-title{
  font-family:'Cinzel',serif;
  font-size:clamp(2.5rem,6vw,4.5rem);
  font-weight:900;letter-spacing:0.1em;text-align:center;
}
.go-title.defeat{
  background:linear-gradient(135deg,var(--red),#fc8181);
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;
  filter:drop-shadow(0 0 30px rgba(229,62,62,0.5));
}
.go-title.victory{
  background:linear-gradient(135deg,var(--gold),var(--gold2));
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;
  filter:drop-shadow(0 0 30px rgba(240,180,41,0.7));
}

.stats-grid{
  display:grid;grid-template-columns:repeat(3,1fr);
  gap:1rem;width:min(500px,90vw);
}
.stat-box{
  background:var(--surface);border:1px solid var(--border);
  padding:1rem;text-align:center;border-radius:4px;
}
.stat-box .num{
  font-family:'Cinzel',serif;
  font-size:1.8rem;font-weight:700;
  color:var(--gold);
}
.stat-box .label{
  font-size:0.7rem;letter-spacing:0.2em;
  text-transform:uppercase;color:var(--muted);
  margin-top:0.2rem;
}

/* ===== CERTIFICATE MODAL ===== */
#certModal{
  position:fixed;inset:0;z-index:200;
  display:none;align-items:center;justify-content:center;
  background:rgba(0,0,0,0.92);backdrop-filter:blur(16px);
  padding:1rem;
}
#certModal.show{display:flex;}

.cert-modal-inner{
  display:flex;flex-direction:column;align-items:center;gap:1.2rem;
  max-height:95vh;overflow-y:auto;
}
.cert-modal-inner::-webkit-scrollbar{width:3px;}
.cert-modal-inner::-webkit-scrollbar-thumb{background:var(--gold);}

/* Certificate canvas wrapper */
#certWrap{
  position:relative;
  box-shadow:0 0 80px rgba(240,180,41,0.25),0 0 0 1px rgba(240,180,41,0.1);
}

.cert-actions{
  display:flex;gap:1rem;flex-wrap:wrap;justify-content:center;
}
.cert-btn{
  padding:0.8rem 2rem;
  font-family:'Cinzel',serif;font-size:0.9rem;font-weight:700;
  letter-spacing:0.15em;text-transform:uppercase;
  border-radius:3px;cursor:pointer;transition:all 0.2s;border:none;
}
.cert-btn.download{
  background:linear-gradient(135deg,#b7791f,var(--gold),#b7791f);
  color:#000;
  box-shadow:0 0 20px rgba(240,180,41,0.4);
}
.cert-btn.download:hover{transform:scale(1.04);box-shadow:0 0 35px rgba(240,180,41,0.6);}
.cert-btn.close-cert{
  background:none;border:1px solid rgba(255,255,255,0.2);
  color:var(--muted);
}
.cert-btn.close-cert:hover{border-color:var(--white);color:var(--white);}

/* View cert button in game-over */
.view-cert-btn{
  padding:0.85rem 2.4rem;
  font-family:'Cinzel',serif;font-size:1rem;font-weight:700;
  letter-spacing:0.2em;text-transform:uppercase;
  background:linear-gradient(135deg,rgba(240,180,41,0.1),rgba(240,180,41,0.2));
  color:var(--gold);
  border:1px solid var(--gold);
  border-radius:3px;cursor:pointer;
  transition:all 0.2s;
  box-shadow:0 0 15px rgba(240,180,41,0.2);
}
.view-cert-btn:hover{
  background:linear-gradient(135deg,rgba(240,180,41,0.2),rgba(240,180,41,0.35));
  box-shadow:0 0 30px rgba(240,180,41,0.4);
  transform:scale(1.03);
}

/* ===== PLAYER NAME INPUT in game over ===== */
.player-name-section{
  display:flex;flex-direction:column;align-items:center;gap:0.5rem;
  width:min(400px,90vw);
}
.player-name-label{
  font-size:0.7rem;letter-spacing:0.25em;text-transform:uppercase;color:var(--muted);
}
.player-name-input{
  width:100%;
  background:var(--surface);border:1px solid var(--border);
  color:var(--white);font-family:'Cinzel',serif;
  font-size:1rem;padding:0.7rem 1.2rem;
  text-align:center;outline:none;border-radius:3px;
  letter-spacing:0.05em;
  transition:border-color 0.2s,box-shadow 0.2s;
}
.player-name-input:focus{
  border-color:var(--gold);
  box-shadow:0 0 15px rgba(240,180,41,0.2);
}
.player-name-input::placeholder{color:var(--muted);}

/* ===== HEADER ===== */
.game-header{
  display:flex;align-items:center;justify-content:space-between;
  padding:0.75rem 2rem;
  border-bottom:1px solid var(--border);
  background:rgba(10,10,15,0.8);
  backdrop-filter:blur(10px);
  flex-shrink:0;
}

.header-logo{
  font-family:'Cinzel',serif;font-size:1.2rem;font-weight:700;
  background:linear-gradient(135deg,var(--gold),var(--gold2));
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;
  letter-spacing:0.1em;
}

.level-badge{
  display:flex;align-items:center;gap:0.5rem;
  background:var(--surface2);
  border:1px solid var(--border);
  padding:0.4rem 1rem;border-radius:100px;
}
.level-label{font-size:0.7rem;letter-spacing:0.2em;color:var(--muted);text-transform:uppercase;}
.level-num{font-family:'Cinzel',serif;font-size:1rem;font-weight:700;color:var(--gold);}

.score-display{
  font-family:'Cinzel',serif;font-size:1rem;
  color:var(--gold);letter-spacing:0.1em;
}

/* ===== ARENA ===== */
.arena{
  flex:1;display:grid;
  grid-template-columns:1fr auto 1fr;
  grid-template-rows:auto 1fr;
  gap:0;
  padding:1rem 2rem;
  overflow:hidden;
  align-items:center;
}

/* Health bars row */
.health-row{
  grid-column:1/-1;
  display:flex;align-items:center;gap:1rem;
  margin-bottom:0.5rem;
}

.fighter-hud{flex:1;}
.fighter-hud.enemy{direction:rtl;}

.fighter-name{
  font-family:'Cinzel',serif;font-size:0.85rem;font-weight:700;
  letter-spacing:0.15em;text-transform:uppercase;
  margin-bottom:0.35rem;
}
.fighter-hud .fighter-name{color:var(--blue2);}
.fighter-hud.enemy .fighter-name{color:var(--red2);}

.hp-bar-wrap{
  height:16px;background:rgba(255,255,255,0.06);
  border-radius:2px;position:relative;
  border:1px solid rgba(255,255,255,0.08);
  overflow:hidden;
}
.hp-bar{
  height:100%;border-radius:2px;
  transition:width 0.3s ease;
  position:relative;
}
.hp-bar.player{
  background:linear-gradient(90deg,#2b6cb0,var(--blue),var(--blue2));
  box-shadow:0 0 10px rgba(66,153,225,0.5);
}
.hp-bar.enemy-bar{
  background:linear-gradient(90deg,var(--red2),var(--red),#9b2c2c);
  box-shadow:0 0 10px rgba(229,62,62,0.5);
  margin-left:auto;
}

/* Shimmer on hp bars */
.hp-bar::after{
  content:'';position:absolute;top:0;left:-100%;
  width:50%;height:100%;
  background:linear-gradient(90deg,transparent,rgba(255,255,255,0.2),transparent);
  animation:barShimmer 2s ease infinite;
}
@keyframes barShimmer{0%{left:-100%;}100%{left:200%;}}

.hp-text{
  font-size:0.7rem;letter-spacing:0.1em;color:var(--muted);
  margin-top:0.2rem;
  direction:ltr;
}
.fighter-hud.enemy .hp-text{text-align:right;}

.vs-divider{
  font-family:'Cinzel',serif;font-size:1.5rem;font-weight:900;
  color:var(--gold);opacity:0.5;
  padding:0 0.5rem;
}

/* ===== CHARACTERS ===== */
.character-zone{
  display:flex;align-items:flex-end;justify-content:center;
  position:relative;height:220px;
}

/* Player warrior */
.warrior{
  position:relative;
  display:flex;flex-direction:column;align-items:center;
  transition:transform 0.1s;
}

.char-sprite{
  font-size:5rem;
  position:relative;z-index:2;
  filter:drop-shadow(0 10px 20px rgba(0,0,0,0.8));
  transition:transform 0.15s;
  animation:breathe 3s ease-in-out infinite;
}
@keyframes breathe{
  0%,100%{transform:translateY(0) scaleY(1);}
  50%{transform:translateY(-4px) scaleY(1.02);}
}

.char-sprite.attack-slash{animation:attackSlash 0.4s ease forwards;}
@keyframes attackSlash{
  0%{transform:translateX(0) rotate(0deg);}
  30%{transform:translateX(40px) rotate(-15deg) scale(1.2);}
  60%{transform:translateX(100px) rotate(-30deg) scale(1.3);}
  100%{transform:translateX(0) rotate(0deg) scale(1);}
}

.char-sprite.take-hit{animation:takeHit 0.3s ease forwards;}
@keyframes takeHit{
  0%{transform:translateX(0);}
  25%{transform:translateX(-20px) rotate(5deg);}
  75%{transform:translateX(10px);}
  100%{transform:translateX(0) rotate(0deg);}
}

.char-sprite.death-anim{animation:deathAnim 1s ease forwards;}
@keyframes deathAnim{
  0%{transform:rotate(0deg);opacity:1;}
  100%{transform:rotate(90deg) translateX(50px);opacity:0;}
}

/* Shadow under characters */
.char-shadow{
  width:60px;height:8px;
  background:rgba(0,0,0,0.6);
  border-radius:50%;
  filter:blur(4px);
  margin-top:4px;
}

/* Aura effects */
.player-aura{
  position:absolute;inset:-20px;
  border-radius:50%;
  pointer-events:none;
  opacity:0;transition:opacity 0.3s;
}
.player-aura.active{opacity:1;}
.player-aura.combo{
  background:radial-gradient(ellipse,rgba(66,153,225,0.3) 0%,transparent 70%);
  animation:auraGlow 0.5s ease infinite alternate;
}
@keyframes auraGlow{0%{transform:scale(0.9);}100%{transform:scale(1.1);}}

.enemy-aura{
  position:absolute;inset:-20px;
  border-radius:50%;pointer-events:none;
  background:radial-gradient(ellipse,rgba(229,62,62,0.15) 0%,transparent 70%);
}

/* ===== MIDDLE ZONE — words ===== */
.middle-zone{
  display:flex;flex-direction:column;
  align-items:center;justify-content:center;
  gap:0.75rem;
  padding:0 1rem;
  min-width:280px;
}

/* Combo streak */
.combo-display{
  font-family:'Cinzel',serif;
  font-size:0.75rem;letter-spacing:0.2em;
  text-transform:uppercase;
  padding:0.3rem 1rem;
  border-radius:100px;
  transition:all 0.3s;
  opacity:0;
}
.combo-display.show{opacity:1;}
.combo-display.streak-low{background:rgba(72,187,120,0.15);border:1px solid var(--green);color:var(--green);}
.combo-display.streak-mid{background:rgba(240,180,41,0.15);border:1px solid var(--gold);color:var(--gold);}
.combo-display.streak-high{background:rgba(229,62,62,0.15);border:1px solid var(--red);color:var(--red);animation:comboPulse 0.3s ease;}
@keyframes comboPulse{0%{transform:scale(1);}50%{transform:scale(1.15);}100%{transform:scale(1);}}

/* Word to type */
.word-container{
  position:relative;
  background:var(--surface);
  border:1px solid var(--border);
  padding:0.8rem 1.8rem;
  border-radius:4px;
  text-align:center;
  min-width:220px;
  box-shadow:0 0 20px rgba(0,0,0,0.5);
}
.word-container::before{
  content:'TYPE THIS';
  position:absolute;top:-10px;left:50%;transform:translateX(-50%);
  font-size:0.55rem;letter-spacing:0.3em;
  background:var(--bg2);padding:0 0.5rem;
  color:var(--muted);text-transform:uppercase;
}

#currentWord{
  font-family:'Rajdhani',sans-serif;
  font-size:2rem;font-weight:700;
  letter-spacing:0.1em;
  color:var(--white);
  display:flex;gap:2px;align-items:center;justify-content:center;
  flex-wrap:wrap;
}

.word-char.correct{color:var(--green);}
.word-char.wrong{color:var(--red);text-decoration:underline;}
.word-char.pending{color:var(--white);}
.word-char.current{
  color:var(--gold);
  animation:cursorBlink 0.7s ease infinite;
  text-shadow:0 0 10px var(--gold);
}
@keyframes cursorBlink{0%,100%{opacity:1;}50%{opacity:0.5;}}

/* Word power indicator */
.word-power{
  display:flex;align-items:center;gap:0.4rem;
  font-size:0.7rem;letter-spacing:0.1em;color:var(--muted);
}
.power-dots{display:flex;gap:3px;}
.power-dot{
  width:8px;height:8px;border-radius:50%;
  background:var(--surface2);transition:background 0.2s;
}
.power-dot.lit{background:var(--gold);box-shadow:0 0 6px var(--gold);}

/* Upcoming words queue */
.word-queue{
  display:flex;gap:0.5rem;align-items:center;
}
.queued-word{
  font-size:0.75rem;color:var(--muted);
  background:var(--surface);
  padding:0.2rem 0.5rem;border-radius:3px;
  border:1px solid rgba(255,255,255,0.05);
}
.queued-label{font-size:0.6rem;letter-spacing:0.15em;color:var(--muted);text-transform:uppercase;}

/* ===== TYPING INPUT ===== */
.input-zone{
  grid-column:1/-1;
  display:flex;flex-direction:column;align-items:center;gap:0.5rem;
}

.input-wrap{
  position:relative;width:min(500px,80vw);
}

#typeInput{
  width:100%;
  background:var(--surface);
  border:1px solid rgba(240,180,41,0.3);
  color:var(--white);
  font-family:'Rajdhani',sans-serif;font-size:1.3rem;font-weight:600;
  padding:0.75rem 1.2rem;
  outline:none;letter-spacing:0.05em;
  border-radius:4px;
  transition:border-color 0.2s,box-shadow 0.2s;
  text-align:center;
}
#typeInput:focus{
  border-color:var(--gold);
  box-shadow:0 0 20px rgba(240,180,41,0.2);
}
#typeInput.input-correct{
  border-color:var(--green)!important;
  box-shadow:0 0 15px rgba(72,187,120,0.4)!important;
  animation:inputCorrect 0.3s ease;
}
@keyframes inputCorrect{0%{transform:scale(1.02);}100%{transform:scale(1);}}
#typeInput.input-wrong{
  border-color:var(--red)!important;
  box-shadow:0 0 15px rgba(229,62,62,0.4)!important;
  animation:inputShake 0.3s ease;
}
@keyframes inputShake{
  0%,100%{transform:translateX(0);}
  25%{transform:translateX(-5px);}
  75%{transform:translateX(5px);}
}

.input-hint{
  font-size:0.7rem;letter-spacing:0.15em;color:var(--muted);
  text-transform:uppercase;text-align:center;
}

/* ===== BOTTOM STATS BAR ===== */
.stats-bar{
  display:flex;align-items:center;justify-content:center;gap:2rem;
  padding:0.6rem 2rem;
  border-top:1px solid var(--border);
  background:rgba(10,10,15,0.8);
  flex-shrink:0;
}
.stat-item{
  display:flex;flex-direction:column;align-items:center;gap:0.1rem;
}
.stat-val{
  font-family:'Cinzel',serif;font-size:1.1rem;font-weight:700;color:var(--gold);
}
.stat-label{font-size:0.6rem;letter-spacing:0.2em;color:var(--muted);text-transform:uppercase;}

/* WPM gauge */
.wpm-gauge{
  position:relative;width:60px;height:60px;
}
.wpm-gauge svg{transform:rotate(-90deg);}
.gauge-bg{fill:none;stroke:rgba(255,255,255,0.06);stroke-width:4;}
.gauge-fill{fill:none;stroke:var(--gold);stroke-width:4;stroke-linecap:round;transition:stroke-dashoffset 0.5s;}
.wpm-val{
  position:absolute;inset:0;
  display:flex;flex-direction:column;align-items:center;justify-content:center;
}
.wpm-num{font-family:'Cinzel',serif;font-size:0.9rem;font-weight:700;color:var(--gold);line-height:1;}
.wpm-lbl{font-size:0.45rem;letter-spacing:0.1em;color:var(--muted);}

/* ===== FLOATING DAMAGE NUMBERS ===== */
.dmg-number{
  position:fixed;pointer-events:none;z-index:20;
  font-family:'Cinzel',serif;font-weight:700;
  animation:dmgFloat 1.2s ease forwards;
}
@keyframes dmgFloat{
  0%{opacity:1;transform:translateY(0) scale(0.5);}
  20%{opacity:1;transform:translateY(-10px) scale(1.2);}
  100%{opacity:0;transform:translateY(-80px) scale(0.8);}
}
.dmg-player{color:var(--blue2);font-size:1.8rem;}
.dmg-enemy{color:var(--red);font-size:1.5rem;}
.dmg-combo{color:var(--gold);font-size:2rem;}
.dmg-miss{color:var(--muted);font-size:1.2rem;}

/* ===== ATTACK EFFECTS ===== */
.slash-effect{
  position:fixed;pointer-events:none;z-index:15;
  font-size:3rem;
  animation:slashAnim 0.5s ease forwards;
}
@keyframes slashAnim{
  0%{opacity:1;transform:scale(0.5) rotate(-30deg);}
  50%{opacity:1;transform:scale(1.5) rotate(0deg);}
  100%{opacity:0;transform:scale(0.3) rotate(30deg);}
}

/* Screen flash */
.screen-flash{
  position:fixed;inset:0;z-index:100;
  pointer-events:none;opacity:0;
  transition:opacity 0.1s;
}
.screen-flash.hit-flash{background:rgba(229,62,62,0.2);}
.screen-flash.attack-flash{background:rgba(66,153,225,0.1);}

/* Enemy attack indicator */
.enemy-attack-bar{
  position:absolute;bottom:0;left:0;right:0;height:3px;
  background:rgba(229,62,62,0.3);overflow:hidden;
}
.enemy-attack-fill{
  height:100%;background:var(--red);
  box-shadow:0 0 8px var(--red);
  transition:width 0.1s linear;
}

/* Level up banner */
.level-up-banner{
  position:fixed;top:20%;left:50%;transform:translateX(-50%);
  z-index:40;pointer-events:none;
  background:linear-gradient(135deg,rgba(19,19,31,0.95),rgba(26,26,46,0.95));
  border:1px solid var(--gold);
  padding:1rem 3rem;text-align:center;
  opacity:0;animation:none;
  box-shadow:0 0 40px rgba(240,180,41,0.3);
}
.level-up-banner.show{animation:levelUp 2s ease forwards;}
@keyframes levelUp{
  0%{opacity:0;transform:translateX(-50%) scale(0.8);}
  15%{opacity:1;transform:translateX(-50%) scale(1.05);}
  70%{opacity:1;transform:translateX(-50%) scale(1);}
  100%{opacity:0;transform:translateX(-50%) scale(0.95);}
}
.lvl-text{
  font-family:'Cinzel',serif;font-size:0.7rem;
  letter-spacing:0.4em;color:var(--muted);margin-bottom:0.3rem;
}
.lvl-num{
  font-family:'Cinzel',serif;font-size:2rem;font-weight:900;
  background:linear-gradient(135deg,var(--gold),var(--gold2));
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;
}

/* Enemy rage indicator */
.rage-bar{
  height:4px;
  background:linear-gradient(90deg,var(--orange),var(--red));
  box-shadow:0 0 8px var(--red);
  transition:width 0.3s;
  border-radius:2px;
  margin-top:4px;
}

/* ===== TITLE PAGE HEADER ===== */
.title-header{
  position:absolute;top:0;left:0;right:0;
  display:flex;align-items:center;justify-content:space-between;
  padding:0 2.5rem;
  height:64px;
  border-bottom:1px solid rgba(240,180,41,0.12);
  background:rgba(10,10,15,0.85);
  backdrop-filter:blur(12px);
  z-index:10;
  flex-shrink:0;
}

.th-logo{
  display:flex;align-items:center;gap:0.55rem;
  text-decoration:none;
}
.th-logo-icon{
  width:34px;height:34px;
  background:linear-gradient(135deg,#b7791f,var(--gold));
  border-radius:6px;
  display:flex;align-items:center;justify-content:center;
  font-size:1.1rem;
  box-shadow:0 0 12px rgba(240,180,41,0.35);
  flex-shrink:0;
}
.th-logo-text{
  font-family:'Cinzel',serif;font-size:1.05rem;font-weight:700;
  letter-spacing:0.12em;
  background:linear-gradient(135deg,var(--gold),var(--gold2));
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;
}

.th-nav{
  display:flex;align-items:center;gap:0.25rem;
  list-style:none;
}
.th-nav li a{
  display:block;padding:0.4rem 0.9rem;
  font-family:'Rajdhani',sans-serif;font-size:0.82rem;font-weight:600;
  letter-spacing:0.12em;text-transform:uppercase;
  color:rgba(247,250,252,0.6);text-decoration:none;
  border-radius:3px;
  transition:color 0.2s,background 0.2s;
  position:relative;
}
.th-nav li a::after{
  content:'';position:absolute;bottom:0;left:50%;transform:translateX(-50%);
  width:0;height:1px;
  background:var(--gold);
  transition:width 0.25s;
}
.th-nav li a:hover{color:var(--gold);}
.th-nav li a:hover::after{width:70%;}

.th-user{
  display:flex;align-items:center;gap:0.6rem;
}
.th-avatar{
  width:32px;height:32px;border-radius:50%;
  background:linear-gradient(135deg,var(--purple),var(--blue));
  display:flex;align-items:center;justify-content:center;
  font-size:0.85rem;font-weight:700;color:#fff;
  border:1px solid rgba(240,180,41,0.3);
  flex-shrink:0;
}
.th-username{
  font-family:'Rajdhani',sans-serif;font-size:0.82rem;font-weight:600;
  letter-spacing:0.05em;color:rgba(247,250,252,0.75);
  white-space:nowrap;
}

/* ===== TITLE PAGE FOOTER ===== */
.title-footer{
  position:absolute;bottom:0;left:0;right:0;
  border-top:1px solid rgba(240,180,41,0.1);
  background:rgba(10,10,15,0.85);
  backdrop-filter:blur(12px);
  padding:0.85rem 2.5rem;
  z-index:10;
  display:flex;align-items:center;justify-content:space-between;
  flex-wrap:wrap;gap:0.5rem;
}

.tf-brand{
  font-family:'Cinzel',serif;font-size:0.75rem;font-weight:700;
  letter-spacing:0.15em;
  background:linear-gradient(135deg,var(--gold),var(--gold2));
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;
}
.tf-tagline{
  font-size:0.65rem;letter-spacing:0.08em;
  color:var(--muted);margin-top:0.1rem;
}

.tf-links{
  display:flex;gap:1.5rem;list-style:none;align-items:center;
}
.tf-links a{
  font-family:'Rajdhani',sans-serif;font-size:0.72rem;font-weight:600;
  letter-spacing:0.1em;text-transform:uppercase;
  color:rgba(247,250,252,0.4);text-decoration:none;
  transition:color 0.2s;
}
.tf-links a:hover{color:var(--gold);}

.tf-copy{
  font-size:0.65rem;letter-spacing:0.05em;
  color:rgba(113,128,150,0.6);
  text-align:right;
}

/* ===== RESPONSIVE ===== */
@media (max-width:640px){
  .th-nav{display:none;}
  .th-logo-text{font-size:0.9rem;}
  .tf-links{gap:0.75rem;}
}
@media (max-height:600px){
  .char-sprite{font-size:3.5rem;}
  .character-zone{height:160px;}
  #currentWord{font-size:1.6rem;}
}

/* ===== PAGE MODALS ===== */
.page-modal{
  position:fixed;inset:0;z-index:80;
  display:none;flex-direction:column;
  background:linear-gradient(180deg,#0a0a0f 0%,#0f0f1a 100%);
  overflow-y:auto;animation:pageIn 0.3s ease;
}
.page-modal.open{display:flex;}
@keyframes pageIn{from{opacity:0;transform:translateY(12px);}to{opacity:1;transform:translateY(0);}}
.page-modal::-webkit-scrollbar{width:3px;}
.page-modal::-webkit-scrollbar-thumb{background:var(--gold);}

.pm-header{
  display:flex;align-items:center;justify-content:space-between;
  padding:1rem 2.5rem;border-bottom:1px solid var(--border);
  background:rgba(10,10,15,0.95);backdrop-filter:blur(10px);
  position:sticky;top:0;z-index:5;flex-shrink:0;
}
.pm-back{
  display:flex;align-items:center;gap:0.5rem;background:none;
  border:1px solid rgba(240,180,41,0.25);color:var(--gold);
  font-family:'Rajdhani',sans-serif;font-size:0.78rem;font-weight:600;
  letter-spacing:0.12em;text-transform:uppercase;
  padding:0.4rem 1rem;border-radius:3px;cursor:pointer;transition:all 0.2s;
}
.pm-back:hover{background:rgba(240,180,41,0.08);}
.pm-logo-inline{
  font-family:'Cinzel',serif;font-size:0.95rem;font-weight:700;
  letter-spacing:0.15em;
  background:linear-gradient(135deg,var(--gold),var(--gold2));
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;
}
.pm-spacer{width:90px;}
.pm-battle-btn{
  padding:0.4rem 1.2rem;
  font-family:'Rajdhani',sans-serif;font-size:0.78rem;font-weight:700;
  letter-spacing:0.12em;text-transform:uppercase;
  background:linear-gradient(135deg,#b7791f,var(--gold));
  color:#000;border:none;border-radius:3px;cursor:pointer;
  transition:all 0.2s;
  box-shadow:0 0 12px rgba(240,180,41,0.3);
  white-space:nowrap;
}
.pm-battle-btn:hover{transform:scale(1.04);box-shadow:0 0 20px rgba(240,180,41,0.5);}
.pm-body{padding:2.5rem 3rem 3rem;max-width:860px;margin:0 auto;width:100%;}

.pm-section-title{
  font-family:'Cinzel',serif;font-size:clamp(1.6rem,3vw,2.1rem);font-weight:700;
  background:linear-gradient(135deg,var(--gold),var(--gold2));
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;
  margin-bottom:0.4rem;letter-spacing:0.05em;
}
.pm-divider{
  width:80px;height:2px;
  background:linear-gradient(90deg,var(--gold),transparent);
  margin-bottom:1.8rem;border-radius:2px;
}
.pm-card{
  background:var(--surface);border:1px solid var(--border);
  border-radius:6px;padding:1.5rem 1.8rem;margin-bottom:1.2rem;
  transition:border-color 0.2s;
}
.pm-card:hover{border-color:rgba(240,180,41,0.3);}
.pm-card-title{
  font-family:'Cinzel',serif;font-size:1rem;font-weight:700;
  color:var(--gold);margin-bottom:0.5rem;letter-spacing:0.05em;
}
.pm-card p,.pm-card li{font-size:0.87rem;color:rgba(247,250,252,0.68);line-height:1.78;}

.pm-hero{
  background:linear-gradient(135deg,rgba(240,180,41,0.06),rgba(159,122,234,0.06));
  border:1px solid var(--border);border-radius:8px;
  padding:2rem 2.2rem;margin-bottom:2rem;
  display:flex;gap:2rem;align-items:center;
}
.pm-hero-icon{font-size:3.5rem;flex-shrink:0;filter:drop-shadow(0 0 20px rgba(240,180,41,0.4));}
.pm-hero h2{font-family:'Cinzel',serif;font-size:1.35rem;font-weight:700;color:var(--white);margin-bottom:0.5rem;}
.pm-hero p{font-size:0.87rem;color:rgba(247,250,252,0.65);line-height:1.75;}

.pm-stats-row{display:grid;grid-template-columns:repeat(3,1fr);gap:1rem;margin:1.5rem 0;}
.pm-stat{background:var(--surface2);border:1px solid var(--border);border-radius:6px;padding:1.1rem;text-align:center;}
.pm-stat-num{font-family:'Cinzel',serif;font-size:1.6rem;font-weight:700;color:var(--gold);}
.pm-stat-label{font-size:0.67rem;letter-spacing:0.15em;color:var(--muted);text-transform:uppercase;margin-top:0.2rem;}

.pm-steps{list-style:none;display:flex;flex-direction:column;gap:0.9rem;}
.pm-steps li{
  display:flex;gap:1rem;align-items:flex-start;
  background:var(--surface);border:1px solid var(--border);
  border-radius:6px;padding:1rem 1.4rem;
}
.pm-step-num{
  width:26px;height:26px;border-radius:50%;flex-shrink:0;
  background:linear-gradient(135deg,#b7791f,var(--gold));
  display:flex;align-items:center;justify-content:center;
  font-family:'Cinzel',serif;font-size:0.78rem;font-weight:700;color:#000;margin-top:1px;
}
.pm-steps li p{font-size:0.86rem;color:rgba(247,250,252,0.7);line-height:1.65;}
.pm-steps li strong{color:var(--gold);}

.pm-table{width:100%;border-collapse:collapse;margin-top:0.5rem;}
.pm-table th{
  font-family:'Cinzel',serif;font-size:0.68rem;letter-spacing:0.2em;
  text-transform:uppercase;color:var(--gold);
  padding:0.75rem 1rem;border-bottom:1px solid var(--border);text-align:left;
}
.pm-table td{
  padding:0.85rem 1rem;border-bottom:1px solid rgba(240,180,41,0.06);
  font-size:0.84rem;color:rgba(247,250,252,0.75);transition:background 0.15s;
}
.pm-table tr:hover td{background:rgba(240,180,41,0.03);}
.rank-1 td:first-child{color:#ffd700;font-weight:700;}
.rank-2 td:first-child{color:#c0c0c0;font-weight:700;}
.rank-3 td:first-child{color:#cd7f32;font-weight:700;}

.ceo-profile{
  display:flex;gap:2rem;align-items:flex-start;
  background:linear-gradient(135deg,rgba(240,180,41,0.05),rgba(66,153,225,0.04));
  border:1px solid var(--border);border-radius:8px;padding:2rem;margin-bottom:1.5rem;
}
.ceo-avatar{
  width:90px;height:90px;border-radius:50%;flex-shrink:0;
  background:linear-gradient(135deg,var(--purple),var(--blue));
  display:flex;align-items:center;justify-content:center;font-size:2.4rem;
  border:3px solid rgba(240,180,41,0.4);box-shadow:0 0 30px rgba(240,180,41,0.2);
}
.ceo-info h3{font-family:'Cinzel',serif;font-size:1.3rem;font-weight:700;color:var(--white);margin-bottom:0.2rem;}
.ceo-role{color:var(--gold);font-size:0.78rem;letter-spacing:0.15em;text-transform:uppercase;margin-bottom:0.6rem;}
.ceo-bio{font-size:0.86rem;color:rgba(247,250,252,0.68);line-height:1.8;}

.skills-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:0.75rem;margin-top:1rem;}
.skill-tag{
  background:var(--surface2);border:1px solid var(--border);
  border-radius:4px;padding:0.5rem 0.9rem;
  font-size:0.78rem;color:rgba(247,250,252,0.7);
  display:flex;align-items:center;gap:0.5rem;
}
.skill-tag .si{color:var(--gold);}

.edu-card{
  background:var(--surface);border:1px solid var(--border);
  border-left:3px solid var(--gold);border-radius:0 6px 6px 0;
  padding:1.2rem 1.5rem;margin-bottom:0.9rem;
}
.edu-degree{font-family:'Cinzel',serif;font-size:0.92rem;font-weight:700;color:var(--gold);}
.edu-inst{font-size:0.82rem;color:rgba(247,250,252,0.85);margin-top:0.2rem;font-weight:600;}
.edu-detail{font-size:0.77rem;color:var(--muted);margin-top:0.25rem;line-height:1.65;}

.footer-section{margin-bottom:2rem;}
.footer-section h3{font-family:'Cinzel',serif;font-size:1rem;font-weight:700;color:var(--gold);margin-bottom:0.7rem;letter-spacing:0.08em;}
.footer-section p,.footer-section li{font-size:0.84rem;color:rgba(247,250,252,0.65);line-height:1.8;}
.footer-section ul{list-style:none;padding:0;}
.footer-section ul li{padding:0.2rem 0;}
.footer-section ul li::before{content:'✦  ';color:var(--gold);font-size:0.65rem;}
.contact-email{
  display:inline-flex;align-items:center;gap:0.5rem;
  background:rgba(240,180,41,0.08);border:1px solid rgba(240,180,41,0.3);
  color:var(--gold);padding:0.55rem 1.3rem;border-radius:4px;
  font-size:0.85rem;font-weight:600;text-decoration:none;
  margin-top:0.6rem;transition:all 0.2s;
}
.contact-email:hover{background:rgba(240,180,41,0.16);}
</style>
</head>
<body>

<div class="bg-layer"></div>
<div class="bg-grid"></div>
<canvas id="particleCanvas"></canvas>

<!-- Screen flash effect -->
<div class="screen-flash" id="screenFlash"></div>

<!-- Level Up Banner -->
<div class="level-up-banner" id="levelBanner">
  <div class="lvl-text">⚔ LEVEL UP ⚔</div>
  <div class="lvl-num" id="levelBannerNum">2</div>
</div>

<!-- TITLE SCREEN -->
<div id="titleScreen">

  <!-- Header -->
  <header class="title-header">
    <a href="#" class="th-logo">
      <div class="th-logo-icon">⚔</div>
      <span class="th-logo-text">TYPE WARRIOR</span>
    </a>
    <ul class="th-nav">
      <li><a href="#" onclick="openPage('home');return false;">Home</a></li>
      <li><a href="#" onclick="openPage('leaderboard');return false;">Leaderboard</a></li>
      <li><a href="#" onclick="openPage('howtoplay');return false;">How to Play</a></li>
      <li><a href="#" onclick="openPage('about');return false;">About</a></li>
    </ul>
    <div class="th-user">
      <div class="th-avatar">S</div>
    </div>
  </header>

  <div class="title-glow">TYPE WARRIOR</div>
  <div class="title-sub">⚔ Battle with Words ⚔</div>
  <div class="title-divider"></div>
  <div class="difficulty-select">
    <button class="diff-btn easy" onclick="selectDiff('easy',this)">Squire</button>
    <button class="diff-btn medium selected" onclick="selectDiff('medium',this)">Knight</button>
    <button class="diff-btn hard" onclick="selectDiff('hard',this)">Champion</button>
    <button class="diff-btn insane" onclick="selectDiff('insane',this)">Warlord</button>
  </div>
  <button class="start-btn" onclick="startGame()">⚔ ENTER BATTLE ⚔</button>
  <div class="title-instructions">
    <strong>Type the words</strong> to attack the enemy<br>
    Faster typing = stronger attacks · Combos = massive damage<br>
    Don't let the enemy reach <strong>0 HP</strong> — fight or perish!
  </div>

  <!-- Footer -->
  <footer class="title-footer">
    <div>
      <div class="tf-brand">⚔ TYPE WARRIOR</div>
      <div class="tf-tagline">Master your keyboard. Conquer your enemies.</div>
    </div>
    <ul class="tf-links">
      <li><a href="#" onclick="openPage('privacy');return false;">Privacy</a></li>
      <li><a href="#" onclick="openPage('terms');return false;">Terms</a></li>
      <li><a href="#" onclick="openPage('support');return false;">Support</a></li>
      <li><a href="#" onclick="openPage('contact');return false;">Contact</a></li>
    </ul>
    <div class="tf-copy">
      © 2025 Type Warrior Academy<br>
      CEO: Syed Shahzaib Ali
    </div>
  </footer>

</div>

<!-- ===== HOME PAGE ===== -->
<div class="page-modal" id="page-home">
  <div class="pm-header">
    <button class="pm-back" onclick="closePage()">← Back to Menu</button>
    <span class="pm-logo-inline">⚔ TYPE WARRIOR</span>
    <button class="pm-battle-btn" onclick="closePage();startGame()">⚔ Enter Battle</button>
  </div>
  <div class="pm-body">
    <div class="pm-hero">
      <div class="pm-hero-icon">⚔</div>
      <div>
        <h2>Welcome to Type Warrior</h2>
        <p>Type Warrior is the ultimate typing battle game where your keyboard is your weapon. Face off against fearsome enemies, unleash devastating combos, and prove your mastery of the written word. Every keystroke counts — the faster and more accurate you type, the harder you strike.</p>
      </div>
    </div>

    <!-- Difficulty selector embedded in Home -->
    <div class="pm-section-title" style="font-size:1.1rem;">Choose Your Difficulty</div>
    <div class="pm-divider"></div>
    <div style="display:flex;gap:1rem;flex-wrap:wrap;margin-bottom:1.5rem;">
      <button class="diff-btn easy"   onclick="selectDiff('easy',this)">⚔ Squire</button>
      <button class="diff-btn medium selected" onclick="selectDiff('medium',this)">⚔ Knight</button>
      <button class="diff-btn hard"   onclick="selectDiff('hard',this)">⚔ Champion</button>
      <button class="diff-btn insane" onclick="selectDiff('insane',this)">⚔ Warlord</button>
    </div>
    <div style="text-align:center;margin-bottom:2rem;">
      <button class="start-btn" onclick="closePage();startGame()">⚔ ENTER BATTLE ⚔</button>
    </div>

    <div class="pm-section-title">Why Type Warrior?</div>
    <div class="pm-divider"></div>
    <div class="pm-card">
      <div class="pm-card-title">⚡ Improve Your Typing Speed</div>
      <p>Traditional typing tutors are boring. Type Warrior wraps skill-building in an exciting combat game. You won't even realize you're practicing — you'll be too busy defeating enemies. Regular play measurably improves your words-per-minute over time.</p>
    </div>
    <div class="pm-card">
      <div class="pm-card-title">🎯 Build Real Accuracy</div>
      <p>Accuracy matters in battle. Missed words increase enemy rage and trigger harder counterattacks. The game teaches you to slow down when needed, strike with precision, and build the muscle memory of a true typing warrior.</p>
    </div>
    <div class="pm-card">
      <div class="pm-card-title">🔥 Combo System & Progression</div>
      <p>Chain consecutive correct words to build devastating combos. Reach a ×10 combo and your damage multiplies dramatically. As you level up, enemies grow stronger and faster — keeping you challenged at every stage of your journey.</p>
    </div>
    <div class="pm-card">
      <div class="pm-card-title">🏆 Earn Your Certificate</div>
      <p>At the end of every battle, receive a personalized certificate of achievement signed by CEO Syed Shahzaib Ali. Your rank — from Apprentice Warrior to Grandmaster — reflects your true typing performance. Download and share it proudly.</p>
    </div>
    <div class="pm-stats-row">
      <div class="pm-stat"><div class="pm-stat-num">4</div><div class="pm-stat-label">Difficulty Modes</div></div>
      <div class="pm-stat"><div class="pm-stat-num">∞</div><div class="pm-stat-label">Words to Master</div></div>
      <div class="pm-stat"><div class="pm-stat-num">5★</div><div class="pm-stat-label">Rank System</div></div>
    </div>
  </div>
</div>

<!-- ===== LEADERBOARD PAGE ===== -->
<div class="page-modal" id="page-leaderboard">
  <div class="pm-header">
    <button class="pm-back" onclick="closePage()">← Back to Menu</button>
    <span class="pm-logo-inline">🏆 Leaderboard</span>
    <button class="pm-battle-btn" onclick="closePage();startGame()">⚔ Enter Battle</button>
  </div>
  <div class="pm-body">
    <div class="pm-section-title">Hall of Warriors</div>
    <div class="pm-divider"></div>
    <div class="pm-card" style="margin-bottom:1.5rem;">
      <p>The greatest warriors who have proven their typing mastery in battle. Rankings are based on score, WPM, and accuracy. Reach the top by defeating enemies at higher difficulties with blazing speed and perfect precision.</p>
    </div>

    <table class="pm-table">
      <thead>
        <tr>
          <th>Rank</th>
          <th>Warrior</th>
          <th>Score</th>
          <th>WPM</th>
          <th>Accuracy</th>
          <th>Difficulty</th>
          <th>Title</th>
        </tr>
      </thead>
      <tbody>
        <tr class="rank-1"><td>🥇 1</td><td>Syed Shahzaib</td><td>48,750</td><td>94</td><td>98%</td><td>Warlord</td><td>Grandmaster Warrior</td></tr>
        <tr class="rank-2"><td>🥈 2</td><td>AliStrikez</td><td>41,200</td><td>82</td><td>96%</td><td>Warlord</td><td>Grandmaster Warrior</td></tr>
        <tr class="rank-3"><td>🥉 3</td><td>TypeBlade_99</td><td>35,680</td><td>76</td><td>93%</td><td>Champion</td><td>Elite Warrior</td></tr>
        <tr><td>4</td><td>KeyStormPK</td><td>29,400</td><td>68</td><td>91%</td><td>Champion</td><td>Elite Warrior</td></tr>
        <tr><td>5</td><td>SwiftFingers</td><td>24,100</td><td>58</td><td>88%</td><td>Knight</td><td>Skilled Warrior</td></tr>
        <tr><td>6</td><td>NightBladePro</td><td>19,850</td><td>52</td><td>85%</td><td>Knight</td><td>Skilled Warrior</td></tr>
        <tr><td>7</td><td>CodeWarriorX</td><td>16,200</td><td>44</td><td>82%</td><td>Squire</td><td>Brave Warrior</td></tr>
        <tr><td>8</td><td>Raza_Types</td><td>12,500</td><td>38</td><td>79%</td><td>Squire</td><td>Brave Warrior</td></tr>
        <tr><td>9</td><td>WarriorBorn</td><td>9,100</td><td>30</td><td>75%</td><td>Squire</td><td>Apprentice Warrior</td></tr>
        <tr><td>10</td><td>NoviceSword</td><td>6,400</td><td>22</td><td>70%</td><td>Squire</td><td>Apprentice Warrior</td></tr>
      </tbody>
    </table>

    <div class="pm-card" style="margin-top:1.5rem;">
      <div class="pm-card-title">🎯 How to Climb the Ranks</div>
      <p>Play on higher difficulty modes to earn more points per word. Build long combo streaks — each combo multiplies your score. Maintain high accuracy to avoid rage-fueled enemy counterattacks that break your streak. Complete battles quickly for time bonuses.</p>
    </div>
  </div>
</div>

<!-- ===== HOW TO PLAY PAGE ===== -->
<div class="page-modal" id="page-howtoplay">
  <div class="pm-header">
    <button class="pm-back" onclick="closePage()">← Back to Menu</button>
    <span class="pm-logo-inline">📖 How to Play</span>
    <button class="pm-battle-btn" onclick="closePage();startGame()">⚔ Enter Battle</button>
  </div>
  <div class="pm-body">
    <div class="pm-section-title">Master the Art of Combat</div>
    <div class="pm-divider"></div>

    <div class="pm-hero">
      <div class="pm-hero-icon">🧙‍♂️</div>
      <div>
        <h2>Your Keyboard Is Your Sword</h2>
        <p>Type Warrior is simple to learn but thrilling to master. Words appear on your screen — type them correctly to strike the enemy. The faster you type, the harder each blow lands. Build combos for devastating power attacks. Survive the enemy's constant assault and emerge victorious.</p>
      </div>
    </div>

    <div class="pm-section-title" style="font-size:1.1rem;">Step-by-Step Guide</div>
    <div class="pm-divider" style="margin-bottom:1.2rem;"></div>
    <ul class="pm-steps">
      <li><div class="pm-step-num">1</div><p><strong>Choose Your Difficulty</strong> — Select Squire (beginner), Knight (medium), Champion (hard), or Warlord (insane). Each level increases enemy speed, damage, and word complexity.</p></li>
      <li><div class="pm-step-num">2</div><p><strong>Click "Enter Battle"</strong> — The game begins immediately. A word appears in the center panel. Your cursor starts at the first letter highlighted in gold.</p></li>
      <li><div class="pm-step-num">3</div><p><strong>Type the Word</strong> — Type each letter carefully. Correct letters turn green; mistakes turn red. The input box glows green on correct completion, red on error.</p></li>
      <li><div class="pm-step-num">4</div><p><strong>Confirm with Enter or Space</strong> — Once you finish typing, press Enter or Space to confirm. A correct word triggers your warrior's attack animation and deals damage to the enemy.</p></li>
      <li><div class="pm-step-num">5</div><p><strong>Build Combo Streaks</strong> — Type consecutive correct words to build a combo. At ×5 combo you deal bonus damage with a special effect. At ×10 combo you unleash a critical hit with gold particle explosions.</p></li>
      <li><div class="pm-step-num">6</div><p><strong>Watch the Enemy Attack Bar</strong> — The red bar beneath your warrior fills over time. When it's full, the enemy strikes automatically. Type faster to deal more damage before the counterattack lands.</p></li>
      <li><div class="pm-step-num">7</div><p><strong>Enemy Rage System</strong> — When you miss words or take hits, the enemy enters a rage state (shown by the orange bar). Enraged enemies attack faster and deal more damage. Recover by typing correctly.</p></li>
      <li><div class="pm-step-num">8</div><p><strong>Level Up</strong> — Every 10 correctly typed words triggers a level up. Your HP partially restores, but the enemy grows stronger and faster. Higher levels mean bigger score multipliers.</p></li>
      <li><div class="pm-step-num">9</div><p><strong>Win or Survive</strong> — Reduce the enemy's HP to zero to claim victory. If your HP reaches zero, you are defeated. Either way, collect your certificate and show the world your warrior spirit.</p></li>
    </ul>

    <div class="pm-stats-row" style="margin-top:1.5rem;">
      <div class="pm-stat"><div class="pm-stat-num">×10</div><div class="pm-stat-label">Max Combo Bonus</div></div>
      <div class="pm-stat"><div class="pm-stat-num">+15%</div><div class="pm-stat-label">HP Heal per Level</div></div>
      <div class="pm-stat"><div class="pm-stat-num">2×</div><div class="pm-stat-label">Max WPM Damage Mult.</div></div>
    </div>

    <div class="pm-card" style="margin-top:0.5rem;">
      <div class="pm-card-title">💡 Pro Tips</div>
      <p>
        • Don't rush — accuracy matters more than raw speed at first.<br>
        • Watch the upcoming word queue to mentally prepare your next word.<br>
        • If you feel overwhelmed, correct a word slowly to break the enemy's momentum.<br>
        • Higher WPM dramatically increases your damage — practice daily to improve.<br>
        • Press Escape at any time to return to the main menu.
      </p>
    </div>
  </div>
</div>

<!-- ===== ABOUT PAGE ===== -->
<div class="page-modal" id="page-about">
  <div class="pm-header">
    <button class="pm-back" onclick="closePage()">← Back to Menu</button>
    <span class="pm-logo-inline">👤 About</span>
    <button class="pm-battle-btn" onclick="closePage();startGame()">⚔ Enter Battle</button>
  </div>
  <div class="pm-body">
    <div class="pm-section-title">About Type Warrior</div>
    <div class="pm-divider"></div>

    <div class="pm-card" style="margin-bottom:1.5rem;">
      <p>Type Warrior is an innovative educational gaming platform designed to transform the way people learn to type. Born from the belief that skill-building should be exciting, the game fuses the discipline of typing practice with the thrill of real-time combat. It was built to serve students, professionals, and anyone who wants to type faster — without the boredom of traditional typing software.</p>
    </div>

    <div class="pm-section-title" style="font-size:1.2rem;">Meet the CEO</div>
    <div class="pm-divider"></div>

    <div class="ceo-profile">
      <img src="data:image/jpeg;base64,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zzzzzzzzzzyyv0eWIHQCVW7xPCeZUZwr/wA8888888888888888888888888880TYwntAGf2pTNQXeAeS4R78888888888888888888888888888rDCUOvpOue5SXzPD8VSDE88888888888888888888888888888sBrUV7ZHMwSLq6SyIrcsB088888888888888888888888888888sh+ANu2e/oNm1vGXHcgfW8888888888888888888888888888884RfxVS24hVkgvYNG3NJ88888888888888888888888888888888vQm3omgzn0CY2W6wg988888888888888888888888888888888q6H+rlWsIVKvmNmHW8888888888888888888888888888888888P7/Sld0NC1Yy3Ra38888888888888888888888888888888888lhRZuQSzw4WfKGx00888888888888888888888888888888888tYGBPKmU3JhM3/Z3d0888888888888888888888888888888888uhDsNz8IyBdH/AH9nG/8Azzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzw2a3Y2FbY8lchHrlHsHTTzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzxxXtF/FD2ourP1sUnH8TL/AM88888888888888888888888888888NiPcueF1EPNSEPDuxr3jx3+8888888888888888888888888888KPdV9WYhOZF7yWvwJScCNF/bz888888888888888888888883AKOk/p6AIwlu/0dzmxuTY5t8PlsD888888888888888888854lMf8Rb+h6L8WdxxT7pvq+hlp1G8n7D88888888888888880HzqT3U6PUk/3HaJETXFPFlQWqnLZcLPFx888888888888888EhR5ELSEV4KYiJ6mAGuwKCt7KGqXrPqmAqA888888888888888jfvqoMSjY7zjsLZ5Kbbgke97m6CPzuJ3jox88888888888884qRzx+wylD6Lrd84pLOPt1HLB8GOOemVUw4Ew88888888888887GHvmwIeJb4doDHV++fB08YbqixyyCyGiLvcy8888888888888r1fq+Mko9JUrqJulA1TvtDgmzNRpU33/SGY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"
           alt="Syed Shahzaib Ali"
           style="width:110px;height:110px;border-radius:50%;object-fit:cover;object-position:top center;flex-shrink:0;border:3px solid rgba(240,180,41,0.5);box-shadow:0 0 30px rgba(240,180,41,0.3);" />
      <div class="ceo-info">
        <h3>Syed Shahzaib Ali</h3>
        <div class="ceo-role">Chief Executive Officer · Founder · Developer</div>
        <div class="ceo-bio">Syed Shahzaib Ali is a passionate technologist, developer, and entrepreneur from Pakistan. Currently pursuing a Bachelor of Science in Artificial Intelligence at CUST (Capital University of Science and Technology), Islamabad, he combines deep technical expertise with a creative vision for building products that educate and entertain. Shahzaib founded Type Warrior Academy with the mission to make typing education accessible, engaging, and genuinely fun for everyone — from school students to working professionals.</div>
      </div>
    </div>

    <div class="pm-section-title" style="font-size:1.1rem;">Education</div>
    <div class="pm-divider" style="margin-bottom:1rem;"></div>

    <div class="edu-card">
      <div class="edu-degree">BS Artificial Intelligence</div>
      <div class="edu-inst">Capital University of Science and Technology (CUST) — Islamabad, Pakistan</div>
      <div class="edu-detail">Studying the core disciplines of modern AI including Machine Learning, Deep Learning, Natural Language Processing, Computer Vision, and Data Science. Applying AI principles to build intelligent, adaptive user experiences in software products like Type Warrior.</div>
    </div>
    <div class="edu-card">
      <div class="edu-degree">Intermediate (Computer Science)</div>
      <div class="edu-inst">Federal Board — Pakistan</div>
      <div class="edu-detail">Strong foundation in Computer Science, Mathematics, and Physics — fuelling a career built on programming, analytical thinking, and technology-driven problem-solving.</div>
    </div>

    <div class="pm-section-title" style="font-size:1.1rem;margin-top:1.5rem;">Skills & Expertise</div>
    <div class="pm-divider" style="margin-bottom:1rem;"></div>

    <div class="pm-card">
      <div class="pm-card-title">Technical Skills</div>
      <div class="skills-grid">
        <div class="skill-tag"><span class="si">🤖</span> Artificial Intelligence & ML</div>
        <div class="skill-tag"><span class="si">🐍</span> Python Programming</div>
        <div class="skill-tag"><span class="si">🌐</span> Web Development (HTML/CSS/JS)</div>
        <div class="skill-tag"><span class="si">🎮</span> Game Design & Development</div>
      </div>
    </div>

    <div class="pm-card">
      <div class="pm-card-title">Leadership & Soft Skills</div>
      <div class="skills-grid">
        <div class="skill-tag"><span class="si">🚀</span> Entrepreneurship & Innovation</div>
        <div class="skill-tag"><span class="si">🎯</span> Product Vision & Strategy</div>
        <div class="skill-tag"><span class="si">🤝</span> Team Leadership</div>
        <div class="skill-tag"><span class="si">✍️</span> Creative Content Design</div>
      </div>
    </div>

    <div class="pm-card" style="margin-top:0.3rem;">
      <div class="pm-card-title">🌟 Vision</div>
      <p>"I believe that technology should not just automate tasks — it should elevate human potential. Type Warrior is my proof of concept that learning and gaming can coexist beautifully. My goal is to build AI-powered educational tools that make a real difference in people's lives, starting here in Pakistan and reaching the world."<br><br><em style="color:var(--gold)">— Syed Shahzaib Ali, CEO</em></p>
    </div>
  </div>
</div>

<!-- ===== PRIVACY PAGE ===== -->
<div class="page-modal" id="page-privacy">
  <div class="pm-header">
    <button class="pm-back" onclick="closePage()">← Back to Menu</button>
    <span class="pm-logo-inline">🔒 Privacy Policy</span>
    <button class="pm-battle-btn" onclick="closePage();startGame()">⚔ Enter Battle</button>
  </div>
  <div class="pm-body">
    <div class="pm-section-title">Privacy Policy</div>
    <div class="pm-divider"></div>
    <div class="footer-section">
      <h3>Your Privacy Matters</h3>
      <p>At Type Warrior Academy, we are committed to protecting your personal information. This Privacy Policy outlines what data we collect, how we use it, and the choices you have. We believe in complete transparency with our users.</p>
    </div>
    <div class="footer-section">
      <h3>Data We Collect</h3>
      <ul>
        <li>Game performance data (WPM, accuracy, scores) — stored locally in your browser only</li>
        <li>Player name entered for certificate generation — never transmitted to any server</li>
        <li>No account creation is required — we do not collect email addresses or passwords</li>
        <li>No cookies are used for tracking or advertising purposes</li>
      </ul>
    </div>
    <div class="footer-section">
      <h3>How We Use Your Data</h3>
      <p>All game data is processed entirely within your browser (client-side). No data is sent to external servers. Your typing statistics, scores, and name are used only to display your in-game results and generate your downloadable certificate. This data is cleared when you close or refresh the page.</p>
    </div>
    <div class="footer-section">
      <h3>Third-Party Services</h3>
      <p>Type Warrior loads fonts from Google Fonts (fonts.googleapis.com). This is the only external resource used. Google's standard font delivery privacy policy applies. No advertising networks, analytics trackers, or third-party cookies are used in this application.</p>
    </div>
    <div class="footer-section">
      <h3>Children's Privacy</h3>
      <p>Type Warrior is suitable for all ages. We do not knowingly collect personal information from children under 13. Since no data is collected at all, the game is fully safe for students and young learners.</p>
    </div>
    <div class="footer-section">
      <h3>Contact for Privacy Concerns</h3>
      <p>If you have any questions about this Privacy Policy, please reach out to us directly:</p>
      <a class="contact-email" href="mailto:shahzaibalibukhari14@gmail.com">📧 shahzaibalibukhari14@gmail.com</a>
    </div>
  </div>
</div>

<!-- ===== TERMS PAGE ===== -->
<div class="page-modal" id="page-terms">
  <div class="pm-header">
    <button class="pm-back" onclick="closePage()">← Back to Menu</button>
    <span class="pm-logo-inline">📜 Terms of Use</span>
    <button class="pm-battle-btn" onclick="closePage();startGame()">⚔ Enter Battle</button>
  </div>
  <div class="pm-body">
    <div class="pm-section-title">Terms of Use</div>
    <div class="pm-divider"></div>
    <div class="footer-section">
      <h3>Acceptance of Terms</h3>
      <p>By playing Type Warrior, you agree to these Terms of Use. These terms govern your use of the game and all related features. Type Warrior is provided as a free educational gaming experience by Type Warrior Academy, founded and operated by Syed Shahzaib Ali.</p>
    </div>
    <div class="footer-section">
      <h3>Permitted Use</h3>
      <ul>
        <li>You may play Type Warrior freely for personal, educational, or recreational purposes</li>
        <li>You may download and share your certificate of achievement for personal use</li>
        <li>You may share screenshots and game results on social media</li>
        <li>Students and teachers may use Type Warrior as an educational tool in classrooms</li>
      </ul>
    </div>
    <div class="footer-section">
      <h3>Prohibited Activities</h3>
      <ul>
        <li>You may not reverse-engineer, copy, or redistribute the game's source code for commercial purposes without written permission</li>
        <li>You may not attempt to manipulate leaderboard rankings through automated means</li>
        <li>You may not represent Type Warrior certificates as official credentials from accredited institutions</li>
      </ul>
    </div>
    <div class="footer-section">
      <h3>Intellectual Property</h3>
      <p>Type Warrior, its design, code, visual elements, and certificate design are the intellectual property of Syed Shahzaib Ali and Type Warrior Academy. All rights reserved © 2025.</p>
    </div>
    <div class="footer-section">
      <h3>Disclaimer</h3>
      <p>Type Warrior is provided "as is" for educational and entertainment purposes. While we strive to ensure accuracy and a smooth experience, Type Warrior Academy makes no warranties regarding uninterrupted access or error-free performance. The game is run entirely in-browser and requires no installation.</p>
    </div>
    <div class="footer-section">
      <h3>Questions About These Terms</h3>
      <p>For any questions regarding these terms, contact us at:</p>
      <a class="contact-email" href="mailto:shahzaibalibukhari14@gmail.com">📧 shahzaibalibukhari14@gmail.com</a>
    </div>
  </div>
</div>

<!-- ===== SUPPORT PAGE ===== -->
<div class="page-modal" id="page-support">
  <div class="pm-header">
    <button class="pm-back" onclick="closePage()">← Back to Menu</button>
    <span class="pm-logo-inline">🛡 Support</span>
    <button class="pm-battle-btn" onclick="closePage();startGame()">⚔ Enter Battle</button>
  </div>
  <div class="pm-body">
    <div class="pm-section-title">Support Center</div>
    <div class="pm-divider"></div>
    <div class="pm-card" style="margin-bottom:1.5rem;">
      <p>We're here to help! Whether you have a technical issue, a question about the game, or feedback to share — our support team is ready to assist. Type Warrior is a passion project dedicated to making typing education fun, and your experience matters to us deeply.</p>
    </div>

    <div class="footer-section">
      <h3>Frequently Asked Questions</h3>
    </div>
    <div class="pm-card">
      <div class="pm-card-title">Why isn't the game responding to my typing?</div>
      <p>Make sure the typing input field is focused (click inside the text box once). The game auto-focuses the input, but clicking elsewhere on the page can shift focus. If on mobile, tap the input field to bring up your keyboard.</p>
    </div>
    <div class="pm-card">
      <div class="pm-card-title">My certificate won't download — what should I do?</div>
      <p>Ensure your browser allows file downloads from this page. The certificate is generated as a PNG image using Canvas technology and downloaded directly to your device. Try a modern browser like Chrome, Edge, or Firefox for best results.</p>
    </div>
    <div class="pm-card">
      <div class="pm-card-title">The sound effects aren't working.</div>
      <p>Sound requires a user interaction (click or keypress) before browsers allow audio. Once you start typing or click a button, audio will activate automatically. If sound is still absent, check your browser's site permissions and ensure the tab is not muted.</p>
    </div>
    <div class="pm-card">
      <div class="pm-card-title">Can I play on mobile?</div>
      <p>Yes! Type Warrior is browser-based and works on mobile devices. Tap the input field to open your keyboard and start typing. For the best experience, we recommend playing on a physical keyboard on a desktop or laptop.</p>
    </div>
    <div class="pm-card">
      <div class="pm-card-title">How is WPM (Words Per Minute) calculated?</div>
      <p>WPM is calculated as the total number of correctly completed words divided by the elapsed time in minutes since the game started. It updates in real time as you play.</p>
    </div>

    <div class="footer-section" style="margin-top:1.5rem;">
      <h3>Still Need Help?</h3>
      <p>If your issue isn't covered above, reach out directly to our support team. We typically respond within 24 hours.</p>
      <a class="contact-email" href="mailto:shahzaibalibukhari14@gmail.com">📧 shahzaibalibukhari14@gmail.com</a>
    </div>
  </div>
</div>

<!-- ===== CONTACT PAGE ===== -->
<div class="page-modal" id="page-contact">
  <div class="pm-header">
    <button class="pm-back" onclick="closePage()">← Back to Menu</button>
    <span class="pm-logo-inline">✉ Contact Us</span>
    <button class="pm-battle-btn" onclick="closePage();startGame()">⚔ Enter Battle</button>
  </div>
  <div class="pm-body">
    <div class="pm-section-title">Get in Touch</div>
    <div class="pm-divider"></div>

    <div class="pm-hero">
      <div class="pm-hero-icon">✉</div>
      <div>
        <h2>We'd Love to Hear from You</h2>
        <p>Whether you have a suggestion to improve the game, a collaboration proposal, a bug to report, or just want to say hello — we welcome every message. Type Warrior was built by a developer who cares deeply about the community.</p>
      </div>
    </div>

    <div class="footer-section">
      <h3>📧 Email (Primary Contact)</h3>
      <p>Send us a message any time. We respond to all genuine inquiries within 24 hours on working days.</p>
      <a class="contact-email" href="mailto:shahzaibalibukhari14@gmail.com">📧 shahzaibalibukhari14@gmail.com</a>
    </div>

    <div class="footer-section" style="margin-top:1rem;">
      <h3>👤 CEO — Direct Line</h3>
      <div class="ceo-profile" style="margin-bottom:0;">
        <div class="ceo-avatar" style="width:60px;height:60px;font-size:1.8rem;">👨‍💻</div>
        <div class="ceo-info">
          <h3 style="font-size:1.05rem;">Syed Shahzaib Ali</h3>
          <div class="ceo-role">CEO & Founder, Type Warrior Academy</div>
          <div class="ceo-bio" style="margin-top:0.4rem;">BS AI Student · CUST Islamabad · Developer & Entrepreneur<br>
          For partnerships, collaborations, academic inquiries, or serious feedback — feel free to email directly.</div>
        </div>
      </div>
    </div>

    <div class="footer-section" style="margin-top:1.5rem;">
      <h3>What to Include in Your Message</h3>
      <ul>
        <li>Your name and how you use Type Warrior</li>
        <li>A clear description of your question, bug, or suggestion</li>
        <li>Your browser and device type (for technical issues)</li>
        <li>Screenshots if relevant to a visual bug</li>
      </ul>
    </div>

    <div class="pm-card" style="margin-top:0.5rem;">
      <div class="pm-card-title">🤝 Collaboration & Partnership</div>
      <p>Are you a school, university, or educational platform interested in integrating Type Warrior into your curriculum or platform? We're open to partnerships that expand access to quality typing education. Reach out at <strong style="color:var(--gold)">shahzaibalibukhari14@gmail.com</strong> with the subject line "Partnership Inquiry".</p>
    </div>
  </div>
</div>

<!-- GAME OVER SCREEN -->
<div id="gameOver">
  <div class="go-title" id="goTitle">DEFEAT</div>
  <div class="title-divider"></div>
  <div class="stats-grid">
    <div class="stat-box"><div class="num" id="goWpm">0</div><div class="label">Best WPM</div></div>
    <div class="stat-box"><div class="num" id="goAccuracy">0%</div><div class="label">Accuracy</div></div>
    <div class="stat-box"><div class="num" id="goScore">0</div><div class="label">Score</div></div>
    <div class="stat-box"><div class="num" id="goWords">0</div><div class="label">Words Typed</div></div>
    <div class="stat-box"><div class="num" id="goCombo">0</div><div class="label">Best Combo</div></div>
    <div class="stat-box"><div class="num" id="goLevel">1</div><div class="label">Level Reached</div></div>
  </div>
  <div class="player-name-section">
    <div class="player-name-label">✦ Enter your name for the certificate ✦</div>
    <input class="player-name-input" id="playerNameInput" type="text" placeholder="Your Name" maxlength="40"/>
  </div>
  <div style="display:flex;gap:1rem;flex-wrap:wrap;justify-content:center;">
    <button class="start-btn" onclick="restartGame()">⚔ PLAY AGAIN</button>
    <button class="view-cert-btn" onclick="openCert()">🏆 View Certificate</button>
  </div>
</div>

<!-- CERTIFICATE MODAL -->
<div id="certModal">
  <div class="cert-modal-inner">
    <div id="certWrap">
      <canvas id="certCanvas"></canvas>
    </div>
    <div class="cert-actions">
      <button class="cert-btn download" onclick="downloadCert()">⬇ Download Certificate</button>
      <button class="cert-btn close-cert" onclick="closeCert()">✕ Close</button>
    </div>
  </div>
</div>

<!-- MAIN GAME -->
<div id="app">
  <!-- Header -->
  <div class="game-header">
    <div class="header-logo">⚔ TYPE WARRIOR</div>
    <div class="level-badge">
      <span class="level-label">Level</span>
      <span class="level-num" id="levelNum">1</span>
    </div>
    <div class="score-display">Score: <span id="scoreDisplay">0</span></div>
  </div>

  <!-- Arena -->
  <div class="arena">

    <!-- Health Bars -->
    <div class="health-row">
      <div class="fighter-hud">
        <div class="fighter-name">⚔ Aldric the Brave</div>
        <div class="hp-bar-wrap">
          <div class="hp-bar player" id="playerHpBar" style="width:100%"></div>
        </div>
        <div class="hp-text" id="playerHpText">100 / 100</div>
      </div>
      <div class="vs-divider">VS</div>
      <div class="fighter-hud enemy">
        <div class="fighter-name">💀 Dark Overlord</div>
        <div class="hp-bar-wrap">
          <div class="hp-bar enemy-bar" id="enemyHpBar" style="width:100%"></div>
        </div>
        <div class="hp-text" id="enemyHpText">100 / 100</div>
        <div class="rage-bar" id="rageBar" style="width:0%"></div>
      </div>
    </div>

    <!-- Player character -->
    <div class="character-zone">
      <div class="warrior" id="playerWarrior">
        <div class="player-aura" id="playerAura"></div>
        <div class="char-sprite" id="playerSprite">🧙‍♂️</div>
        <div class="char-shadow"></div>
        <div class="enemy-attack-bar">
          <div class="enemy-attack-fill" id="enemyAttackBar" style="width:0%"></div>
        </div>
      </div>
    </div>

    <!-- Middle: word zone -->
    <div class="middle-zone">
      <div class="combo-display" id="comboDisplay">🔥 COMBO x1</div>

      <div class="word-container">
        <div id="currentWord"></div>
      </div>

      <div class="power-dots">
        <div class="power-dot" id="pd1"></div>
        <div class="power-dot" id="pd2"></div>
        <div class="power-dot" id="pd3"></div>
        <div class="power-dot" id="pd4"></div>
        <div class="power-dot" id="pd5"></div>
      </div>

      <div class="word-queue">
        <span class="queued-label">Next:</span>
        <span class="queued-word" id="q1">—</span>
        <span class="queued-word" id="q2">—</span>
      </div>
    </div>

    <!-- Enemy character -->
    <div class="character-zone">
      <div class="warrior" id="enemyWarrior">
        <div class="enemy-aura"></div>
        <div class="char-sprite" id="enemySprite">💀</div>
        <div class="char-shadow"></div>
      </div>
    </div>

    <!-- Input -->
    <div class="input-zone">
      <div class="input-wrap">
        <input type="text" id="typeInput" placeholder="Start typing..." autocomplete="off" autocorrect="off" spellcheck="false"/>
      </div>
      <div class="input-hint">⌨ Type the word · Press Enter or Space to confirm</div>
    </div>
  </div>

  <!-- Stats bar -->
  <div class="stats-bar">
    <div class="stat-item">
      <div class="wpm-gauge">
        <svg viewBox="0 0 60 60" width="60" height="60">
          <circle class="gauge-bg" cx="30" cy="30" r="24"/>
          <circle class="gauge-fill" id="wpmGauge" cx="30" cy="30" r="24"
            stroke-dasharray="150.8" stroke-dashoffset="150.8"/>
        </svg>
        <div class="wpm-val">
          <div class="wpm-num" id="wpmDisplay">0</div>
          <div class="wpm-lbl">WPM</div>
        </div>
      </div>
    </div>
    <div class="stat-item">
      <div class="stat-val" id="accuracyDisplay">100%</div>
      <div class="stat-label">Accuracy</div>
    </div>
    <div class="stat-item">
      <div class="stat-val" id="comboStatDisplay">0</div>
      <div class="stat-label">Combo</div>
    </div>
    <div class="stat-item">
      <div class="stat-val" id="wordsDisplay">0</div>
      <div class="stat-label">Words</div>
    </div>
    <div class="stat-item">
      <div class="stat-val" id="bestComboDisplay">0</div>
      <div class="stat-label">Best Combo</div>
    </div>
  </div>
</div>

<script>
// ==================== WORD LISTS ====================
const wordLists = {
  easy: ['cat','dog','run','jump','fire','sky','hand','fast','blue','star','rock','free','bold','glow','win','hunt','rage','dark','pure','wild'],
  medium: ['sword','brave','flame','storm','quest','honor','blood','chaos','power','steel','valor','magic','night','curse','light','forge','blade','swift','glory','spell'],
  hard: ['champion','warlord','fortress','betrayal','conquest','onslaught','vengeance','mystical','shattered','consumed','obliterate','relentless','ferocious','indomitable'],
  insane: ['insurmountable','unparalleled','catastrophic','annihilation','indestructible','transcendence','overwhelming','devastation','mercilessly','extraordinary']
};

const diffSettings = {
  easy:   {enemySpeed:8000, enemyDmg:8,  wordCount:1, maxHp:120, baseScore:10, wordsPool:'easy'},
  medium: {enemySpeed:5000, enemyDmg:12, wordCount:1, maxHp:100, baseScore:15, wordsPool:'medium'},
  hard:   {enemySpeed:3500, enemyDmg:16, wordCount:1, maxHp:80,  baseScore:20, wordsPool:'hard'},
  insane: {enemySpeed:2500, enemyDmg:22, wordCount:1, maxHp:60,  baseScore:30, wordsPool:'insane'}
};

// ==================== STATE ====================
let state = {
  running: false,
  difficulty: 'medium',
  playerHp: 100, playerMaxHp: 100,
  enemyHp: 100, enemyMaxHp: 100,
  level: 1, score: 0,
  combo: 0, bestCombo: 0,
  totalWords: 0, correctWords: 0, totalChars: 0, wrongChars: 0,
  wpm: 0, startTime: null, lastWordTime: null,
  wordQueue: [], currentWord: '', typedSoFar: '',
  enemyAttackTimer: null, enemyAttackProgress: 0,
  enemyAttackInterval: 5000,
  rage: 0, // 0-100 enemy rage
  wordsForLevel: 10, wordsThisLevel: 0,
  // WPM tracking
  wpmHistory: [], lastMinuteWords: []
};

// ==================== AUDIO ====================
const AudioCtx = window.AudioContext || window.webkitAudioContext;
let audioCtx = null;

function getAudioCtx() {
  if (!audioCtx) audioCtx = new AudioCtx();
  return audioCtx;
}

function playSound(type) {
  try {
    const ctx = getAudioCtx();
    const osc = ctx.createOscillator();
    const gain = ctx.createGain();
    osc.connect(gain); gain.connect(ctx.destination);

    const sounds = {
      attack:  {freq:[440,550,660], dur:0.12, type:'sawtooth', vol:0.15},
      comboHit:{freq:[550,700,880], dur:0.15, type:'square',   vol:0.18},
      bigHit:  {freq:[880,1100],    dur:0.2,  type:'sawtooth', vol:0.25},
      takeDmg: {freq:[200,150,100], dur:0.2,  type:'sawtooth', vol:0.2},
      combo5:  {freq:[660,880,1100,1320], dur:0.25, type:'square', vol:0.2},
      levelUp: {freq:[440,550,660,880], dur:0.4, type:'sine', vol:0.2},
      miss:    {freq:[200,180],     dur:0.1,  type:'square',   vol:0.08},
      victory: {freq:[523,659,784,1047], dur:0.5, type:'sine', vol:0.2},
      defeat:  {freq:[220,196,175,131], dur:0.6, type:'sawtooth', vol:0.15},
      key:     {freq:[800],         dur:0.03, type:'sine',     vol:0.04}
    };

    const s = sounds[type] || sounds.key;
    osc.type = s.type;
    gain.gain.setValueAtTime(s.vol, ctx.currentTime);

    s.freq.forEach((f, i) => {
      osc.frequency.setValueAtTime(f, ctx.currentTime + i * (s.dur / s.freq.length));
    });

    gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + s.dur);
    osc.start(ctx.currentTime);
    osc.stop(ctx.currentTime + s.dur);
  } catch(e) {}
}

// ==================== PARTICLES ====================
const canvas = document.getElementById('particleCanvas');
const pctx = canvas.getContext('2d');
let particles = [];

function resizeCanvas() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
}
resizeCanvas();
window.addEventListener('resize', resizeCanvas);

function spawnParticle(x, y, color, count=6, speed=3) {
  for (let i = 0; i < count; i++) {
    const angle = Math.random() * Math.PI * 2;
    const spd = (Math.random() * speed + 1);
    particles.push({
      x, y,
      vx: Math.cos(angle) * spd,
      vy: Math.sin(angle) * spd - Math.random() * 2,
      life: 1,
      decay: Math.random() * 0.03 + 0.02,
      size: Math.random() * 6 + 2,
      color
    });
  }
}

function animateParticles() {
  pctx.clearRect(0, 0, canvas.width, canvas.height);
  particles = particles.filter(p => p.life > 0);
  particles.forEach(p => {
    p.x += p.vx; p.y += p.vy;
    p.vy += 0.1;
    p.life -= p.decay;
    pctx.globalAlpha = p.life;
    pctx.fillStyle = p.color;
    pctx.beginPath();
    pctx.arc(p.x, p.y, p.size * p.life, 0, Math.PI * 2);
    pctx.fill();
  });
  pctx.globalAlpha = 1;
  requestAnimationFrame(animateParticles);
}
animateParticles();

// ==================== UI HELPERS ====================
function floatDamage(x, y, text, cls) {
  const el = document.createElement('div');
  el.className = `dmg-number ${cls}`;
  el.textContent = text;
  el.style.left = x + 'px';
  el.style.top = y + 'px';
  document.body.appendChild(el);
  setTimeout(() => el.remove(), 1200);
}

function flashScreen(cls) {
  const f = document.getElementById('screenFlash');
  f.className = `screen-flash ${cls}`;
  f.style.opacity = '1';
  setTimeout(() => f.style.opacity = '0', 150);
}

function showSlashEffect(x, y, emoji) {
  const el = document.createElement('div');
  el.className = 'slash-effect';
  el.textContent = emoji;
  el.style.left = x + 'px';
  el.style.top = y + 'px';
  document.body.appendChild(el);
  setTimeout(() => el.remove(), 500);
}

// ==================== WORD MANAGEMENT ====================
function getWords() {
  const pool = diffSettings[state.difficulty].wordsPool;
  const extra = state.level > 3 ? 'hard' : null;
  let words = [...wordLists[pool]];
  if (extra && extra !== pool) words = words.concat(wordLists[extra]);
  return words;
}

function generateWord() {
  const words = getWords();
  let word;
  do { word = words[Math.floor(Math.random() * words.length)]; }
  while (word === state.currentWord);
  return word;
}

function fillQueue() {
  while (state.wordQueue.length < 3) {
    state.wordQueue.push(generateWord());
  }
}

function nextWord() {
  fillQueue();
  state.currentWord = state.wordQueue.shift();
  state.typedSoFar = '';
  fillQueue();
  renderWord();
  updateQueueDisplay();
  document.getElementById('typeInput').value = '';
  state.lastWordTime = Date.now();
}

function renderWord() {
  const container = document.getElementById('currentWord');
  container.innerHTML = '';
  [...state.currentWord].forEach((ch, i) => {
    const span = document.createElement('span');
    span.className = 'word-char';
    span.textContent = ch;
    if (i < state.typedSoFar.length) {
      span.className += state.typedSoFar[i] === ch ? ' correct' : ' wrong';
    } else if (i === state.typedSoFar.length) {
      span.className += ' current';
    } else {
      span.className += ' pending';
    }
    container.appendChild(span);
  });
}

function updateQueueDisplay() {
  document.getElementById('q1').textContent = state.wordQueue[0] || '—';
  document.getElementById('q2').textContent = state.wordQueue[1] || '—';
}

// ==================== POWER DOTS ====================
function updatePowerDots(correct, total) {
  const ratio = correct / total;
  for (let i = 1; i <= 5; i++) {
    const dot = document.getElementById('pd' + i);
    dot.classList.toggle('lit', ratio >= i / 5);
  }
}

// ==================== HEALTH BARS ====================
function updateHealthBars() {
  const pPct = Math.max(0, state.playerHp / state.playerMaxHp * 100);
  const ePct = Math.max(0, state.enemyHp / state.enemyMaxHp * 100);

  document.getElementById('playerHpBar').style.width = pPct + '%';
  document.getElementById('enemyHpBar').style.width = ePct + '%';
  document.getElementById('playerHpText').textContent = `${Math.max(0,Math.round(state.playerHp))} / ${state.playerMaxHp}`;
  document.getElementById('enemyHpText').textContent = `${Math.max(0,Math.round(state.enemyHp))} / ${state.enemyMaxHp}`;

  // Color hp bar based on hp
  const pBar = document.getElementById('playerHpBar');
  if (pPct < 25) pBar.style.background = 'linear-gradient(90deg,#9b2c2c,var(--red))';
  else if (pPct < 50) pBar.style.background = 'linear-gradient(90deg,var(--orange),#ed8936)';
  else pBar.style.background = 'linear-gradient(90deg,#2b6cb0,var(--blue),var(--blue2))';
}

// ==================== WPM ====================
function updateWPM() {
  const now = Date.now();
  const elapsed = (now - state.startTime) / 60000;
  if (elapsed < 0.01) return;
  state.wpm = Math.round(state.correctWords / elapsed);
  const wpm = Math.min(state.wpm, 120);
  document.getElementById('wpmDisplay').textContent = state.wpm;

  // Gauge
  const circumference = 150.8;
  const offset = circumference - (wpm / 120) * circumference;
  document.getElementById('wpmGauge').style.strokeDashoffset = offset;

  // Color gauge by speed
  const gauge = document.getElementById('wpmGauge');
  if (state.wpm > 80) gauge.style.stroke = 'var(--purple)';
  else if (state.wpm > 50) gauge.style.stroke = 'var(--gold)';
  else if (state.wpm > 30) gauge.style.stroke = 'var(--green)';
  else gauge.style.stroke = 'var(--blue)';
}

function updateAccuracy() {
  const total = state.totalChars + state.wrongChars;
  const acc = total === 0 ? 100 : Math.round((state.totalChars / total) * 100);
  document.getElementById('accuracyDisplay').textContent = acc + '%';
  return acc;
}

// ==================== COMBO ====================
function updateComboDisplay() {
  const d = document.getElementById('comboDisplay');
  const cs = document.getElementById('comboStatDisplay');
  cs.textContent = state.combo;
  document.getElementById('bestComboDisplay').textContent = state.bestCombo;

  if (state.combo <= 0) { d.classList.remove('show'); return; }

  d.classList.add('show');
  d.classList.remove('streak-low','streak-mid','streak-high');

  let icon = '⚡';
  if (state.combo >= 10) { d.classList.add('streak-high'); icon = '🔥'; }
  else if (state.combo >= 5) { d.classList.add('streak-mid'); icon = '✨'; }
  else { d.classList.add('streak-low'); }

  d.textContent = `${icon} COMBO ×${state.combo}`;
}

// ==================== ATTACK ====================
function playerAttack() {
  const wpmBonus = Math.min(state.wpm / 60, 2); // up to 2x damage from speed
  const comboBonus = 1 + (state.combo * 0.15);
  const wordLenBonus = state.currentWord.length * 0.5;
  let dmg = Math.round((8 + wordLenBonus) * wpmBonus * comboBonus);
  dmg = Math.max(5, Math.min(dmg, 60));

  state.enemyHp -= dmg;
  state.score += dmg * (state.level) + (state.combo > 3 ? state.combo * 5 : 0);
  document.getElementById('scoreDisplay').textContent = state.score;

  // Animate player attack
  const sprite = document.getElementById('playerSprite');
  sprite.classList.remove('attack-slash');
  void sprite.offsetWidth;
  sprite.classList.add('attack-slash');
  setTimeout(() => sprite.classList.remove('attack-slash'), 400);

  // Animate enemy take hit
  const eSprite = document.getElementById('enemySprite');
  eSprite.classList.remove('take-hit');
  void eSprite.offsetWidth;
  eSprite.classList.add('take-hit');
  setTimeout(() => eSprite.classList.remove('take-hit'), 300);

  // Effects
  const eRect = document.getElementById('enemyWarrior').getBoundingClientRect();
  const ex = eRect.left + eRect.width / 2;
  const ey = eRect.top + eRect.height / 2;

  flashScreen('attack-flash');
  showSlashEffect(ex - 30, ey - 30, state.combo >= 5 ? '⚡' : '💥');

  if (state.combo >= 10) {
    spawnParticle(ex, ey, '#ffd700', 20, 6);
    floatDamage(ex - 20, ey - 40, `💥 ${dmg}!`, 'dmg-combo');
    playSound('bigHit');
  } else if (state.combo >= 5) {
    spawnParticle(ex, ey, '#e53e3e', 12, 4);
    floatDamage(ex - 20, ey - 40, `⚡ ${dmg}`, 'dmg-combo');
    playSound('comboHit');
  } else {
    spawnParticle(ex, ey, '#fc8181', 6, 3);
    floatDamage(ex - 20, ey - 40, `-${dmg}`, 'dmg-enemy');
    playSound('attack');
  }

  // Player aura for combos
  const aura = document.getElementById('playerAura');
  if (state.combo >= 3) {
    aura.className = 'player-aura active combo';
    setTimeout(() => aura.className = 'player-aura', 600);
  }

  updateHealthBars();
  updateWPM();
}

// ==================== ENEMY ATTACK SYSTEM ====================
function startEnemyAttackTimer() {
  clearEnemyTimer();
  state.enemyAttackProgress = 0;
  const settings = diffSettings[state.difficulty];
  const speedMultiplier = Math.max(0.4, 1 - (state.rage / 200));
  state.enemyAttackInterval = settings.enemySpeed * speedMultiplier / state.level;

  const step = 50;
  state.enemyAttackTimer = setInterval(() => {
    if (!state.running) return;
    state.enemyAttackProgress += step;
    const pct = (state.enemyAttackProgress / state.enemyAttackInterval) * 100;
    document.getElementById('enemyAttackBar').style.width = Math.min(pct, 100) + '%';

    if (state.enemyAttackProgress >= state.enemyAttackInterval) {
      enemyAttack();
      state.enemyAttackProgress = 0;
    }
  }, step);
}

function clearEnemyTimer() {
  if (state.enemyAttackTimer) {
    clearInterval(state.enemyAttackTimer);
    state.enemyAttackTimer = null;
  }
}

function enemyAttack() {
  const settings = diffSettings[state.difficulty];
  const rageMult = 1 + (state.rage / 100);
  let dmg = Math.round(settings.enemyDmg * rageMult);
  state.playerHp -= dmg;

  // Player take hit animation
  const sprite = document.getElementById('playerSprite');
  sprite.classList.remove('take-hit');
  void sprite.offsetWidth;
  sprite.classList.add('take-hit');
  setTimeout(() => sprite.classList.remove('take-hit'), 300);

  // Effects
  const pRect = document.getElementById('playerWarrior').getBoundingClientRect();
  const px = pRect.left + pRect.width / 2;
  const py = pRect.top + pRect.height / 2;

  flashScreen('hit-flash');
  spawnParticle(px, py, '#4299e1', 8, 3);
  floatDamage(px - 20, py - 40, `-${dmg}`, 'dmg-enemy');
  playSound('takeDmg');

  // Reset combo on getting hit
  if (state.combo > 0) {
    state.combo = Math.floor(state.combo * 0.5);
    updateComboDisplay();
  }

  // Increase rage slowly
  state.rage = Math.min(100, state.rage + 5);
  updateRageBar();
  updateHealthBars();

  if (state.playerHp <= 0) { endGame(false); }
}

function updateRageBar() {
  document.getElementById('rageBar').style.width = state.rage + '%';
}

// ==================== LEVEL SYSTEM ====================
function checkLevelUp() {
  state.wordsThisLevel++;
  if (state.wordsThisLevel >= state.wordsForLevel) {
    state.level++;
    state.wordsThisLevel = 0;
    state.wordsForLevel = 10 + state.level * 2;
    document.getElementById('levelNum').textContent = state.level;

    // Level up banner
    const banner = document.getElementById('levelBanner');
    document.getElementById('levelBannerNum').textContent = `LEVEL ${state.level}`;
    banner.classList.remove('show');
    void banner.offsetWidth;
    banner.classList.add('show');

    // Restore some HP
    const heal = Math.round(state.playerMaxHp * 0.15);
    state.playerHp = Math.min(state.playerMaxHp, state.playerHp + heal);

    playSound('levelUp');
    spawnParticle(window.innerWidth / 2, window.innerHeight / 2, '#ffd700', 30, 5);
    updateHealthBars();
    startEnemyAttackTimer(); // recalculate interval
  }
}

// ==================== INPUT HANDLING ====================
document.getElementById('typeInput').addEventListener('input', function(e) {
  if (!state.running) return;

  const val = this.value.toLowerCase().trim();
  const target = state.currentWord.toLowerCase();

  state.typedSoFar = val;

  // Track chars
  if (val.length > 0) {
    const lastChar = val[val.length - 1];
    const targetChar = target[val.length - 1];
    if (lastChar === targetChar) {
      state.totalChars++;
    } else {
      state.wrongChars++;
    }
    updateAccuracy();
  }

  renderWord();
  updatePowerDots(
    [...val].filter((c,i) => c === target[i]).length,
    Math.max(val.length, 1)
  );

  // Check complete
  if (val === target) {
    onWordComplete(true);
  }
  playSound('key');
});

document.getElementById('typeInput').addEventListener('keydown', function(e) {
  if (!state.running) return;
  if (e.key === 'Enter' || e.key === ' ') {
    e.preventDefault();
    const val = this.value.toLowerCase().trim();
    if (val === state.currentWord.toLowerCase()) {
      onWordComplete(true);
    } else if (val.length > 0) {
      onWordComplete(false);
    }
  }
});

function onWordComplete(correct) {
  if (correct) {
    state.combo++;
    state.bestCombo = Math.max(state.bestCombo, state.combo);
    state.correctWords++;
    state.totalWords++;

    // Decrease rage on successful word
    state.rage = Math.max(0, state.rage - 3);
    updateRageBar();

    this_flash('input-correct');
    if (state.combo === 5) playSound('combo5');
    else if (state.combo % 10 === 0) playSound('bigHit');

    playerAttack();
    checkLevelUp();
  } else {
    state.combo = 0;
    state.totalWords++;
    state.wrongChars += 3;
    state.rage = Math.min(100, state.rage + 8);
    updateRageBar();

    this_flash('input-wrong');
    playSound('miss');

    // Miss float
    const pRect = document.getElementById('playerWarrior').getBoundingClientRect();
    floatDamage(pRect.left + 30, pRect.top + 30, 'MISS!', 'dmg-miss');
  }

  updateComboDisplay();
  updateAccuracy();
  document.getElementById('wordsDisplay').textContent = state.correctWords;

  if (state.enemyHp <= 0) { endGame(true); return; }

  nextWord();
}

// Detached from method context
let flashTimeout;
function this_flash(cls) {
  const inp = document.getElementById('typeInput');
  inp.classList.remove('input-correct','input-wrong');
  clearTimeout(flashTimeout);
  void inp.offsetWidth;
  inp.classList.add(cls);
  flashTimeout = setTimeout(() => inp.classList.remove(cls), 400);
}

// ==================== GAME FLOW ====================
function selectDiff(diff, btn) {
  state.difficulty = diff;
  document.querySelectorAll('.diff-btn').forEach(b => b.classList.remove('selected'));
  btn.classList.add('selected');
}

function startGame() {
  const s = diffSettings[state.difficulty];
  Object.assign(state, {
    running: true,
    playerHp: s.maxHp, playerMaxHp: s.maxHp,
    enemyHp: 100, enemyMaxHp: 100,
    level: 1, score: 0,
    combo: 0, bestCombo: 0,
    totalWords: 0, correctWords: 0, totalChars: 0, wrongChars: 0,
    wpm: 0, startTime: Date.now(), lastWordTime: Date.now(),
    wordQueue: [], currentWord: '', typedSoFar: '',
    rage: 0, wordsForLevel: 10, wordsThisLevel: 0
  });

  document.getElementById('titleScreen').style.display = 'none';
  document.getElementById('gameOver').classList.remove('show');
  document.getElementById('levelNum').textContent = '1';
  document.getElementById('scoreDisplay').textContent = '0';

  updateHealthBars();
  updateRageBar();
  updateComboDisplay();
  fillQueue();
  nextWord();
  startEnemyAttackTimer();

  document.getElementById('typeInput').focus();
}

function restartGame() {
  clearEnemyTimer();
  document.getElementById('gameOver').classList.remove('show');
  document.getElementById('titleScreen').style.display = 'flex';
}

function endGame(victory) {
  state.running = false;
  clearEnemyTimer();

  const acc = updateAccuracy();
  document.getElementById('goTitle').textContent = victory ? '⚔ VICTORY ⚔' : '💀 DEFEAT 💀';
  document.getElementById('goTitle').className = 'go-title ' + (victory ? 'victory' : 'defeat');
  document.getElementById('goWpm').textContent = state.wpm;
  document.getElementById('goAccuracy').textContent = acc + '%';
  document.getElementById('goScore').textContent = state.score;
  document.getElementById('goWords').textContent = state.correctWords;
  document.getElementById('goCombo').textContent = state.bestCombo;
  document.getElementById('goLevel').textContent = state.level;

  if (victory) {
    playSound('victory');
    for (let i = 0; i < 5; i++) {
      setTimeout(() => {
        spawnParticle(
          Math.random() * window.innerWidth,
          Math.random() * window.innerHeight,
          ['#ffd700','#ff2d55','#0bc5ea','#9f7aea'][Math.floor(Math.random()*4)],
          15, 4
        );
      }, i * 200);
    }
  } else {
    playSound('defeat');
    const sprite = document.getElementById('playerSprite');
    sprite.classList.add('death-anim');
  }

  setTimeout(() => {
    document.getElementById('gameOver').classList.add('show');
  }, 800);
}

// ==================== ENEMY HP SCALING ====================
// Enemy gets more HP as levels increase
setInterval(() => {
  if (!state.running) return;
  if (state.level > 1 && state.enemyHp <= 0) return;
  // Slowly regenerate enemy HP after level changes (boss mechanic)
}, 1000);

// Watch for level to scale enemy HP
let lastLevel = 1;
setInterval(() => {
  if (!state.running) return;
  if (state.level !== lastLevel) {
    lastLevel = state.level;
    state.enemyHp = 100 + state.level * 20;
    state.enemyMaxHp = state.enemyHp;
    updateHealthBars();
  }
}, 500);

// ==================== KEYBOARD SHORTCUT ====================
document.addEventListener('keydown', e => {
  if (e.key === 'Escape' && state.running) {
    state.running = false;
    clearEnemyTimer();
    document.getElementById('titleScreen').style.display = 'flex';
  }
  // Auto-focus input
  if (state.running && e.key.length === 1 && !e.ctrlKey && !e.metaKey) {
    const inp = document.getElementById('typeInput');
    if (document.activeElement !== inp) inp.focus();
  }
});

// ==================== AMBIENT PARTICLES ====================
setInterval(() => {
  if (Math.random() < 0.3) {
    spawnParticle(
      Math.random() * window.innerWidth,
      window.innerHeight + 10,
      `rgba(240,180,41,${Math.random() * 0.3 + 0.1})`,
      1, 0.5
    );
  }
}, 200);

// ==================== CERTIFICATE ====================

function getRank(wpm, accuracy, score) {
  if (wpm >= 80 && accuracy >= 95) return {title:'Grandmaster Warrior', stars:5, color:'#ffd700'};
  if (wpm >= 60 && accuracy >= 90) return {title:'Elite Warrior',       stars:4, color:'#c0c0c0'};
  if (wpm >= 40 && accuracy >= 80) return {title:'Skilled Warrior',     stars:3, color:'#cd7f32'};
  if (wpm >= 25 && accuracy >= 70) return {title:'Brave Warrior',       stars:2, color:'#4299e1'};
  return                                   {title:'Apprentice Warrior',  stars:1, color:'#48bb78'};
}

function openCert() {
  const playerName = document.getElementById('playerNameInput').value.trim() || 'Brave Warrior';
  drawCertificate(playerName);
  document.getElementById('certModal').classList.add('show');
}

function closeCert() {
  document.getElementById('certModal').classList.remove('show');
}

function drawCertificate(playerName) {
  const canvas = document.getElementById('certCanvas');
  const W = 900, H = 636;
  canvas.width = W;
  canvas.height = H;
  canvas.style.width  = Math.min(W, window.innerWidth - 40) + 'px';
  canvas.style.height = 'auto';
  const ctx = canvas.getContext('2d');

  const acc   = parseInt(document.getElementById('goAccuracy').textContent) || 0;
  const wpm   = parseInt(document.getElementById('goWpm').textContent) || 0;
  const score = parseInt(document.getElementById('goScore').textContent) || 0;
  const words = parseInt(document.getElementById('goWords').textContent) || 0;
  const combo = parseInt(document.getElementById('goCombo').textContent) || 0;
  const level = parseInt(document.getElementById('goLevel').textContent) || 1;
  const rank  = getRank(wpm, acc, score);
  const isVictory = document.getElementById('goTitle').classList.contains('victory');
  const diffLabel = state.difficulty.charAt(0).toUpperCase() + state.difficulty.slice(1);
  const date  = new Date().toLocaleDateString('en-US',{year:'numeric',month:'long',day:'numeric'});

  // ── BACKGROUND ──────────────────────────────────────────────
  // Deep parchment gradient
  const bgGrad = ctx.createLinearGradient(0, 0, W, H);
  bgGrad.addColorStop(0,   '#1a1408');
  bgGrad.addColorStop(0.3, '#221a08');
  bgGrad.addColorStop(0.7, '#1e1506');
  bgGrad.addColorStop(1,   '#120e04');
  ctx.fillStyle = bgGrad;
  ctx.fillRect(0, 0, W, H);

  // Subtle texture overlay (crosshatch)
  ctx.save();
  ctx.globalAlpha = 0.04;
  for (let x = 0; x < W; x += 12) {
    ctx.beginPath(); ctx.moveTo(x,0); ctx.lineTo(x,H);
    ctx.strokeStyle = '#f0b429'; ctx.lineWidth = 0.5; ctx.stroke();
  }
  for (let y = 0; y < H; y += 12) {
    ctx.beginPath(); ctx.moveTo(0,y); ctx.lineTo(W,y);
    ctx.strokeStyle = '#f0b429'; ctx.lineWidth = 0.5; ctx.stroke();
  }
  ctx.restore();

  // Radial glow center
  const glow = ctx.createRadialGradient(W/2, H/2, 0, W/2, H/2, 420);
  glow.addColorStop(0,   'rgba(240,180,41,0.07)');
  glow.addColorStop(0.5, 'rgba(240,180,41,0.03)');
  glow.addColorStop(1,   'transparent');
  ctx.fillStyle = glow;
  ctx.fillRect(0, 0, W, H);

  // Corner glows
  [[0,0],[W,0],[0,H],[W,H]].forEach(([cx,cy]) => {
    const cg = ctx.createRadialGradient(cx, cy, 0, cx, cy, 220);
    cg.addColorStop(0, 'rgba(240,180,41,0.06)');
    cg.addColorStop(1, 'transparent');
    ctx.fillStyle = cg;
    ctx.fillRect(0, 0, W, H);
  });

  // ── OUTER ORNATE BORDER ──────────────────────────────────────
  function roundRect(x, y, w, h, r, strokeStyle, lineWidth, dash=[]) {
    ctx.save();
    ctx.beginPath();
    ctx.moveTo(x+r, y);
    ctx.lineTo(x+w-r, y); ctx.quadraticCurveTo(x+w, y, x+w, y+r);
    ctx.lineTo(x+w, y+h-r); ctx.quadraticCurveTo(x+w, y+h, x+w-r, y+h);
    ctx.lineTo(x+r, y+h); ctx.quadraticCurveTo(x, y+h, x, y+h-r);
    ctx.lineTo(x, y+r); ctx.quadraticCurveTo(x, y, x+r, y);
    ctx.closePath();
    ctx.strokeStyle = strokeStyle;
    ctx.lineWidth = lineWidth;
    if (dash.length) ctx.setLineDash(dash);
    ctx.stroke();
    ctx.restore();
  }

  // Outer border (thick gold)
  roundRect(12, 12, W-24, H-24, 6, '#c9a84c', 3);
  // Second border
  roundRect(20, 20, W-40, H-40, 4, 'rgba(240,180,41,0.4)', 1);
  // Inner dashed border
  roundRect(30, 30, W-60, H-60, 3, 'rgba(240,180,41,0.25)', 0.7, [6,4]);

  // ── CORNER ORNAMENTS ──────────────────────────────────────────
  function drawCornerOrnament(x, y, flipX, flipY) {
    ctx.save();
    ctx.translate(x, y);
    ctx.scale(flipX ? -1 : 1, flipY ? -1 : 1);
    ctx.strokeStyle = '#c9a84c';
    ctx.lineWidth = 1.5;
    ctx.globalAlpha = 0.8;

    // Bracket
    ctx.beginPath();
    ctx.moveTo(0, 40); ctx.lineTo(0, 8); ctx.quadraticCurveTo(0, 0, 8, 0); ctx.lineTo(40, 0);
    ctx.stroke();

    // Inner curl
    ctx.beginPath();
    ctx.moveTo(8, 35); ctx.lineTo(8, 14); ctx.quadraticCurveTo(8, 8, 14, 8); ctx.lineTo(35, 8);
    ctx.stroke();

    // Diamond dot
    ctx.fillStyle = '#f0b429';
    ctx.globalAlpha = 0.9;
    ctx.beginPath();
    ctx.moveTo(4, 0); ctx.lineTo(8, 4); ctx.lineTo(4, 8); ctx.lineTo(0, 4);
    ctx.closePath(); ctx.fill();

    ctx.restore();
  }

  drawCornerOrnament(12, 12, false, false);
  drawCornerOrnament(W-12, 12, true, false);
  drawCornerOrnament(12, H-12, false, true);
  drawCornerOrnament(W-12, H-12, true, true);

  // ── TOP EMBLEM / SEAL ─────────────────────────────────────────
  const cx = W / 2;

  // Outer ring of seal
  ctx.save();
  ctx.globalAlpha = 0.9;
  const sealGrad = ctx.createRadialGradient(cx, 78, 0, cx, 78, 44);
  sealGrad.addColorStop(0,   '#3d2e08');
  sealGrad.addColorStop(0.7, '#2a1f05');
  sealGrad.addColorStop(1,   '#1a1408');
  ctx.fillStyle = sealGrad;
  ctx.beginPath(); ctx.arc(cx, 78, 44, 0, Math.PI*2); ctx.fill();

  ctx.strokeStyle = '#c9a84c'; ctx.lineWidth = 2;
  ctx.beginPath(); ctx.arc(cx, 78, 44, 0, Math.PI*2); ctx.stroke();
  ctx.strokeStyle = 'rgba(240,180,41,0.3)'; ctx.lineWidth = 1;
  ctx.beginPath(); ctx.arc(cx, 78, 38, 0, Math.PI*2); ctx.stroke();

  // Seal emoji / sword icon
  ctx.font = '38px serif';
  ctx.textAlign = 'center';
  ctx.textBaseline = 'middle';
  ctx.fillText('⚔', cx, 78);
  ctx.restore();

  // Decorative line through seal
  ctx.save();
  ctx.globalAlpha = 0.6;
  const lineGrad = ctx.createLinearGradient(cx-180, 78, cx+180, 78);
  lineGrad.addColorStop(0, 'transparent');
  lineGrad.addColorStop(0.2, '#c9a84c');
  lineGrad.addColorStop(0.5, '#f0b429');
  lineGrad.addColorStop(0.8, '#c9a84c');
  lineGrad.addColorStop(1, 'transparent');
  ctx.strokeStyle = lineGrad;
  ctx.lineWidth = 1;
  ctx.beginPath(); ctx.moveTo(cx-180, 78); ctx.lineTo(cx-50, 78); ctx.stroke();
  ctx.beginPath(); ctx.moveTo(cx+50, 78); ctx.lineTo(cx+180, 78); ctx.stroke();
  ctx.restore();

  // ── HEADER TEXT ───────────────────────────────────────────────
  // "CERTIFICATE OF"
  ctx.save();
  ctx.textAlign = 'center';
  ctx.fillStyle = '#a07830';
  ctx.font = '600 11px "Georgia", serif';
  ctx.letterSpacing = '0.4em';
  ctx.fillText('C E R T I F I C A T E   O F', cx, 144);
  ctx.restore();

  // "ACHIEVEMENT"
  ctx.save();
  ctx.textAlign = 'center';
  const achieveGrad = ctx.createLinearGradient(cx-160, 0, cx+160, 0);
  achieveGrad.addColorStop(0,   '#b7791f');
  achieveGrad.addColorStop(0.3, '#f0b429');
  achieveGrad.addColorStop(0.5, '#ffd700');
  achieveGrad.addColorStop(0.7, '#f0b429');
  achieveGrad.addColorStop(1,   '#b7791f');
  ctx.fillStyle = achieveGrad;
  ctx.font = 'bold 42px "Georgia", serif';
  ctx.shadowColor = 'rgba(240,180,41,0.5)';
  ctx.shadowBlur = 18;
  ctx.fillText('ACHIEVEMENT', cx, 186);
  ctx.restore();

  // ── DIVIDER ORNAMENT ─────────────────────────────────────────
  function drawDivider(y, w=340) {
    ctx.save();
    const dg = ctx.createLinearGradient(cx-w, y, cx+w, y);
    dg.addColorStop(0, 'transparent');
    dg.addColorStop(0.15, '#c9a84c');
    dg.addColorStop(0.5, '#f0b429');
    dg.addColorStop(0.85, '#c9a84c');
    dg.addColorStop(1, 'transparent');
    ctx.strokeStyle = dg;
    ctx.lineWidth = 1;
    ctx.beginPath(); ctx.moveTo(cx-w, y); ctx.lineTo(cx+w, y); ctx.stroke();

    // Center diamond
    ctx.fillStyle = '#f0b429';
    ctx.globalAlpha = 0.9;
    ctx.beginPath();
    ctx.moveTo(cx, y-5); ctx.lineTo(cx+5, y); ctx.lineTo(cx, y+5); ctx.lineTo(cx-5, y);
    ctx.closePath(); ctx.fill();

    // Small dots
    [-60, 60].forEach(dx => {
      ctx.beginPath();
      ctx.arc(cx+dx, y, 2, 0, Math.PI*2);
      ctx.fill();
    });
    ctx.restore();
  }

  drawDivider(204, 300);

  // ── "THIS IS PRESENTED TO" ────────────────────────────────────
  ctx.save();
  ctx.textAlign = 'center';
  ctx.fillStyle = '#8a6e3a';
  ctx.font = 'italic 13px "Georgia", serif';
  ctx.fillText('This certificate is proudly presented to', cx, 232);
  ctx.restore();

  // ── PLAYER NAME ───────────────────────────────────────────────
  ctx.save();
  ctx.textAlign = 'center';
  const nameGrad = ctx.createLinearGradient(cx-200, 0, cx+200, 0);
  nameGrad.addColorStop(0,   '#c9a84c');
  nameGrad.addColorStop(0.5, '#fff8e1');
  nameGrad.addColorStop(1,   '#c9a84c');
  ctx.fillStyle = nameGrad;
  // Responsive font size
  const nameSize = playerName.length > 22 ? 34 : playerName.length > 14 ? 40 : 46;
  ctx.font = `bold italic ${nameSize}px "Georgia", serif`;
  ctx.shadowColor = 'rgba(240,180,41,0.6)';
  ctx.shadowBlur = 25;
  ctx.fillText(playerName, cx, 285);
  ctx.restore();

  // Underline under name
  ctx.save();
  const ulGrad = ctx.createLinearGradient(cx-160, 0, cx+160, 0);
  ulGrad.addColorStop(0, 'transparent');
  ulGrad.addColorStop(0.5, '#c9a84c');
  ulGrad.addColorStop(1, 'transparent');
  ctx.strokeStyle = ulGrad;
  ctx.lineWidth = 1;
  ctx.beginPath(); ctx.moveTo(cx-160, 302); ctx.lineTo(cx+160, 302); ctx.stroke();
  ctx.restore();

  drawDivider(318, 260);

  // ── RANK BANNER ───────────────────────────────────────────────
  ctx.save();
  ctx.textAlign = 'center';

  // Rank badge background
  const rankBg = ctx.createLinearGradient(cx-130, 335, cx+130, 365);
  rankBg.addColorStop(0, 'transparent');
  rankBg.addColorStop(0.2, 'rgba(240,180,41,0.08)');
  rankBg.addColorStop(0.8, 'rgba(240,180,41,0.08)');
  rankBg.addColorStop(1, 'transparent');
  ctx.fillStyle = rankBg;
  ctx.fillRect(cx-130, 328, 260, 36);

  ctx.fillStyle = rank.color;
  ctx.font = 'bold 18px "Georgia", serif';
  ctx.shadowColor = rank.color;
  ctx.shadowBlur = 12;
  ctx.fillText(`✦ ${rank.title} ✦`, cx, 353);
  ctx.restore();

  // Stars
  ctx.save();
  ctx.textAlign = 'center';
  ctx.font = '16px serif';
  const starStr = '★'.repeat(rank.stars) + '☆'.repeat(5 - rank.stars);
  ctx.fillStyle = '#f0b429';
  ctx.shadowColor = '#ffd700';
  ctx.shadowBlur = 8;
  ctx.fillText(starStr, cx, 376);
  ctx.restore();

  // ── STATS ROW ────────────────────────────────────────────────
  const stats = [
    {label:'WPM',      val: wpm,       icon:'⚡'},
    {label:'Accuracy', val: acc+'%',   icon:'🎯'},
    {label:'Score',    val: score,     icon:'⚔'},
    {label:'Words',    val: words,     icon:'📜'},
    {label:'Best Combo',val: combo,    icon:'🔥'},
    {label:'Level',    val: level,     icon:'🏆'},
  ];

  const sW = (W - 100) / 6;
  const sY = 404;

  stats.forEach((s, i) => {
    const sx = 50 + sW * i + sW / 2;

    // Stat box bg
    ctx.save();
    ctx.fillStyle = 'rgba(240,180,41,0.05)';
    ctx.strokeStyle = 'rgba(240,180,41,0.2)';
    ctx.lineWidth = 1;
    ctx.beginPath();
    ctx.roundRect(sx - sW/2 + 4, sY - 8, sW - 8, 58, 3);
    ctx.fill();
    ctx.stroke();
    ctx.restore();

    ctx.save();
    ctx.textAlign = 'center';
    ctx.font = '14px serif';
    ctx.fillText(s.icon, sx, sY + 10);

    ctx.fillStyle = '#f0b429';
    ctx.font = 'bold 16px "Georgia", serif';
    ctx.shadowColor = 'rgba(240,180,41,0.4)';
    ctx.shadowBlur = 6;
    ctx.fillText(s.val, sx, sY + 30);

    ctx.fillStyle = '#8a6e3a';
    ctx.font = '9px "Georgia", serif';
    ctx.shadowBlur = 0;
    ctx.fillText(s.label.toUpperCase(), sx, sY + 46);
    ctx.restore();
  });

  drawDivider(475, 280);

  // ── FOR EXCELLENCE IN ─────────────────────────────────────────
  ctx.save();
  ctx.textAlign = 'center';
  ctx.fillStyle = '#8a6e3a';
  ctx.font = 'italic 12px "Georgia", serif';
  ctx.fillText(`For demonstrating excellence in typing speed, accuracy, and warrior spirit`, cx, 498);
  ctx.fillText(`on ${diffLabel} difficulty — ${isVictory ? 'achieving glorious victory' : 'battling with extraordinary courage'}`, cx, 516);
  ctx.restore();

  // ── SIGNATURE SECTION ────────────────────────────────────────
  const sigY = 572;

  // Left: date
  ctx.save();
  ctx.textAlign = 'center';
  ctx.fillStyle = '#8a6e3a';
  ctx.font = 'italic 11px "Georgia", serif';
  ctx.fillText('Date of Achievement', 200, sigY - 22);

  // Date underline
  ctx.strokeStyle = 'rgba(240,180,41,0.4)'; ctx.lineWidth = 0.8;
  ctx.beginPath(); ctx.moveTo(120, sigY-8); ctx.lineTo(280, sigY-8); ctx.stroke();

  ctx.fillStyle = '#c9a84c';
  ctx.font = '11px "Georgia", serif';
  ctx.fillText(date, 200, sigY - 2);
  ctx.restore();

  // Center seal
  ctx.save();
  const miniSeal = ctx.createRadialGradient(cx, sigY-12, 0, cx, sigY-12, 26);
  miniSeal.addColorStop(0, 'rgba(240,180,41,0.15)');
  miniSeal.addColorStop(1, 'transparent');
  ctx.fillStyle = miniSeal;
  ctx.beginPath(); ctx.arc(cx, sigY-12, 26, 0, Math.PI*2); ctx.fill();
  ctx.strokeStyle = 'rgba(240,180,41,0.4)'; ctx.lineWidth = 1;
  ctx.beginPath(); ctx.arc(cx, sigY-12, 26, 0, Math.PI*2); ctx.stroke();
  ctx.font = '22px serif';
  ctx.textAlign = 'center';
  ctx.fillText(isVictory ? '🏆' : '⚔', cx, sigY - 6);
  ctx.restore();

  // Right: CEO signature
  ctx.save();
  ctx.textAlign = 'center';

  // Signature stroke (simulate handwriting)
  ctx.strokeStyle = '#c9a84c';
  ctx.lineWidth = 1.5;
  ctx.globalAlpha = 0.7;
  ctx.beginPath();
  ctx.moveTo(620, sigY - 22);
  ctx.bezierCurveTo(630, sigY - 36, 660, sigY - 30, 670, sigY - 22);
  ctx.bezierCurveTo(680, sigY - 14, 700, sigY - 28, 710, sigY - 22);
  ctx.bezierCurveTo(720, sigY - 16, 740, sigY - 28, 750, sigY - 22);
  ctx.stroke();
  // Underline flourish
  ctx.beginPath();
  ctx.moveTo(615, sigY - 16);
  ctx.bezierCurveTo(660, sigY - 10, 700, sigY - 8, 755, sigY - 14);
  ctx.stroke();
  ctx.globalAlpha = 1;

  // Signature underline
  ctx.strokeStyle = 'rgba(240,180,41,0.4)'; ctx.lineWidth = 0.8;
  ctx.beginPath(); ctx.moveTo(620, sigY - 8); ctx.lineTo(780, sigY - 8); ctx.stroke();

  ctx.fillStyle = '#c9a84c';
  ctx.font = 'bold italic 12px "Georgia", serif';
  ctx.shadowColor = 'rgba(240,180,41,0.3)';
  ctx.shadowBlur = 6;
  ctx.fillText('Syed Shahzaib Ali', 700, sigY + 2);

  ctx.fillStyle = '#8a6e3a';
  ctx.font = '9.5px "Georgia", serif';
  ctx.shadowBlur = 0;
  ctx.fillText('Chief Executive Officer', 700, sigY + 16);
  ctx.fillText('Type Warrior Academy', 700, sigY + 29);
  ctx.restore();

  // ── BOTTOM ORB / WATERMARK ────────────────────────────────────
  ctx.save();
  ctx.globalAlpha = 0.06;
  ctx.font = 'bold 120px "Georgia", serif';
  ctx.textAlign = 'center';
  ctx.fillStyle = '#f0b429';
  ctx.fillText('TW', cx, H/2 + 50);
  ctx.restore();
}

function downloadCert() {
  const canvas = document.getElementById('certCanvas');
  // Redraw at full res before download
  const playerName = document.getElementById('playerNameInput').value.trim() || 'Brave Warrior';
  drawCertificate(playerName);

  const link = document.createElement('a');
  link.download = `TypeWarrior_Certificate_${(playerName).replace(/\s+/g,'_')}.png`;
  link.href = canvas.toDataURL('image/png', 1.0);
  link.click();
}

// Close cert modal on backdrop click
document.getElementById('certModal').addEventListener('click', function(e) {
  if (e.target === this) closeCert();
});

// ==================== PAGE NAVIGATION ====================
function openPage(pageId) {
  // Close any open page first
  document.querySelectorAll('.page-modal').forEach(p => p.classList.remove('open'));
  const page = document.getElementById('page-' + pageId);
  if (page) {
    page.classList.add('open');
    page.scrollTop = 0;
  }
}

function closePage() {
  document.querySelectorAll('.page-modal').forEach(p => p.classList.remove('open'));
}

// Close page on backdrop click (clicking the modal itself outside content)
document.querySelectorAll('.page-modal').forEach(modal => {
  modal.addEventListener('click', function(e) {
    if (e.target === this) closePage();
  });
});

// Auto-focus input
document.getElementById('typeInput').addEventListener('blur', () => {
  if (state.running) setTimeout(() => document.getElementById('typeInput').focus(), 10);
});

</script>
</body>
</html>
