# carte-postale-num-rique-

<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Des nouvelles d'ailleurs — Apoutchou Freddy, Fruitpoutchou & ÇApoutchou</title>
<style>
/* RESET */
*, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }
html { scroll-behavior: smooth; overflow-x: hidden; }
body {
  background-color: #1A2E4A;
  color: #FAF0E6;
  font-family: 'Palatino Linotype', 'Palatino', 'Book Antiqua', Georgia, serif;
  font-size: 18px;
  line-height: 1.75;
  overflow-x: hidden;
}

/* CURSEUR */
.cursor-dot {
  width: 6px; height: 6px;
  background-color: #E8845A;
  border-radius: 50%;
  position: fixed;
  pointer-events: none;
  z-index: 9999;
  transition: transform 0.15s ease;
}
.cursor-ring {
  width: 32px; height: 32px;
  border: 1px solid rgba(232,132,90,0.5);
  border-radius: 50%;
  position: fixed;
  pointer-events: none;
  z-index: 9998;
  transition: width 0.3s ease, height 0.3s ease, opacity 0.3s ease;
}

/* CANVAS */
#bg-canvas {
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 100%;
  z-index: 0;
}

/* GRAIN */
.grain {
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  z-index: 1;
  opacity: 0.025;
  pointer-events: none;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  background-size: 200px 200px;
}

/* WRAPPER */
.page-wrapper { position: relative; z-index: 2; }

/* HERO */
.hero {
  min-height: 100vh;
  display: -webkit-box; display: -ms-flexbox; display: flex;
  -webkit-box-orient: vertical; -webkit-box-direction: normal;
  -ms-flex-direction: column; flex-direction: column;
  -webkit-box-align: center; -ms-flex-align: center; align-items: center;
  -webkit-box-pack: center; -ms-flex-pack: center; justify-content: center;
  text-align: center;
  padding: 4rem 2rem;
  position: relative;
}
.hero-inner { position: relative; }

.hero-stamp {
  font-size: 0.65rem;
  letter-spacing: 0.4em;
  text-transform: uppercase;
  color: #D4A96A;
  margin-bottom: 2.5rem;
  opacity: 0;
  -webkit-animation: fadeUp 1s ease forwards 0.3s;
  animation: fadeUp 1s ease forwards 0.3s;
}
.hero-eyebrow {
  font-size: 0.75rem;
  letter-spacing: 0.5em;
  text-transform: uppercase;
  color: #E8A5A0;
  margin-bottom: 1.5rem;
  opacity: 0;
  -webkit-animation: fadeUp 1s ease forwards 0.6s;
  animation: fadeUp 1s ease forwards 0.6s;
}
.hero-title {
  font-family: Georgia, 'Times New Roman', serif;
  font-size: 6rem;
  font-weight: 400;
  line-height: 1.05;
  color: #FAF0E6;
  letter-spacing: -0.01em;
  margin-bottom: 2rem;
  opacity: 0;
  -webkit-animation: fadeUp 1.2s ease forwards 0.9s;
  animation: fadeUp 1.2s ease forwards 0.9s;
}
.hero-title em { font-style: italic; color: #E8845A; }

.hero-subtitle {
  font-size: 1.15rem;
  color: #E8D8C8;
  max-width: 520px;
  font-style: italic;
  margin-bottom: 4rem;
  opacity: 0;
  -webkit-animation: fadeUp 1s ease forwards 1.3s;
  animation: fadeUp 1s ease forwards 1.3s;
}
.hero-scroll {
  display: -webkit-box; display: -ms-flexbox; display: flex;
  -webkit-box-orient: vertical; -webkit-box-direction: normal;
  -ms-flex-direction: column; flex-direction: column;
  -webkit-box-align: center; -ms-flex-align: center; align-items: center;
  gap: 0.5rem;
  color: #E8D8C8;
  font-size: 0.7rem;
  letter-spacing: 0.3em;
  text-transform: uppercase;
  opacity: 0;
  -webkit-animation: fadeUp 1s ease forwards 2s;
  animation: fadeUp 1s ease forwards 2s;
}
.scroll-line {
  width: 1px; height: 50px;
  background: linear-gradient(to bottom, transparent, #E8845A, transparent);
  -webkit-animation: scrollPulse 2s ease-in-out infinite 2.5s;
  animation: scrollPulse 2s ease-in-out infinite 2.5s;
}

/* DIVIDER */
.divider {
  display: -webkit-box; display: -ms-flexbox; display: flex;
  -webkit-box-align: center; -ms-flex-align: center; align-items: center;
  -webkit-box-pack: center; -ms-flex-pack: center; justify-content: center;
  gap: 1.5rem;
  padding: 2rem 0;
  opacity: 0;
}
.divider.visible { -webkit-animation: fadeIn 1s ease forwards; animation: fadeIn 1s ease forwards; }
.divider-line { width: 80px; height: 1px; background: linear-gradient(to right, transparent, #D4A96A); }
.divider-line.right { background: linear-gradient(to left, transparent, #D4A96A); }
.divider-dot { width: 5px; height: 5px; border-radius: 50%; background-color: #D4A96A; }

/* LETTRE */
.letter-section { padding: 6rem 2rem; max-width: 820px; margin: 0 auto; }

.section-label {
  font-size: 0.65rem;
  letter-spacing: 0.5em;
  text-transform: uppercase;
  color: #D4A96A;
  display: block;
  margin-bottom: 3rem;
  text-align: center;
  opacity: 0;
}
.section-label.visible { -webkit-animation: fadeUp 0.8s ease forwards; animation: fadeUp 0.8s ease forwards; }

.letter-card {
  background-color: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.08);
  border-radius: 4px;
  padding: 4rem 4rem 3rem;
  position: relative;
  opacity: 0;
  -webkit-transform: translateY(30px);
  transform: translateY(30px);
}
.letter-card.visible { -webkit-animation: fadeUp 1s ease forwards; animation: fadeUp 1s ease forwards; }
.letter-card::before {
  content: '';
  position: absolute;
  top: -1px; left: 10%;
  width: 80%; height: 1px;
  background: linear-gradient(to right, transparent, #E8845A, transparent);
}

.letter-postmark {
  position: absolute;
  top: 2rem; right: 2.5rem;
  text-align: right;
  font-size: 0.6rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: #E8D8C8;
  opacity: 0.45;
  line-height: 1.8;
}
.letter-postmark span {
  display: block;
  font-size: 0.8rem;
  color: #E8845A;
  opacity: 1;
  font-style: italic;
  letter-spacing: 0.05em;
  margin-bottom: 0.3rem;
}
.letter-greeting { font-size: 1.3rem; font-style: italic; color: #FAF0E6; margin-bottom: 2rem; }

.letter-body { color: #E8D8C8; font-size: 1.05rem; line-height: 1.9; }
.letter-body p {
  margin-bottom: 1.5rem;
  opacity: 0;
  -webkit-transform: translateY(10px);
  transform: translateY(10px);
  -webkit-transition: opacity 0.8s ease, -webkit-transform 0.8s ease;
  transition: opacity 0.8s ease, transform 0.8s ease;
}
.letter-body p.visible { opacity: 1; -webkit-transform: translateY(0); transform: translateY(0); }
.letter-body p.freddy       { color: #FAF0E6; }
.letter-body p.capoutchou   { color: #F2C9C5; }
.letter-body p.fruitpoutchou{ color: #D4A96A; font-style: italic; }

.letter-signature {
  margin-top: 2.5rem;
  padding-top: 2rem;
  border-top: 1px solid rgba(255,255,255,0.07);
}
.sig-names {
  display: -webkit-box; display: -ms-flexbox; display: flex;
  -ms-flex-wrap: wrap; flex-wrap: wrap;
  gap: 2rem;
  margin-top: 1rem;
}
.sig-name { font-family: Georgia, serif; font-size: 1.1rem; font-style: italic; line-height: 1.3; }
.sig-name.freddy     { color: #FAF0E6; }
.sig-name.capoutchou { color: #F2C9C5; }
.sig-name.fruit      { color: #D4A96A; }

/* QUOTIDIEN */
.daily-section { padding: 6rem 2rem; max-width: 1100px; margin: 0 auto; }
.daily-title {
  font-family: Georgia, serif;
  font-size: 2.6rem;
  font-weight: 400;
  text-align: center;
  color: #FAF0E6;
  margin-bottom: 1rem;
  opacity: 0;
}
.daily-title.visible { -webkit-animation: fadeUp 0.8s ease forwards; animation: fadeUp 0.8s ease forwards; }
.daily-subtitle {
  text-align: center;
  color: #E8D8C8;
  font-style: italic;
  font-size: 1rem;
  margin-bottom: 4rem;
  opacity: 0;
}
.daily-subtitle.visible { -webkit-animation: fadeUp 0.8s ease 0.2s forwards; animation: fadeUp 0.8s ease 0.2s forwards; }

.daily-grid {
  display: -ms-grid; display: grid;
  -ms-grid-columns: (minmax(290px, 1fr))[auto-fit];
  grid-template-columns: repeat(auto-fit, minmax(290px, 1fr));
  gap: 1.5rem;
}
.daily-card {
  background-color: rgba(255,255,255,0.03);
  border: 1px solid rgba(255,255,255,0.07);
  border-radius: 3px;
  padding: 2.5rem 2rem 3.5rem;
  position: relative;
  overflow: hidden;
  cursor: pointer;
  opacity: 0;
  -webkit-transform: translateY(20px);
  transform: translateY(20px);
  -webkit-transition: background-color 0.4s ease, border-color 0.4s ease, -webkit-transform 0.4s ease;
  transition: background-color 0.4s ease, border-color 0.4s ease, transform 0.4s ease;
}
.daily-card.visible { -webkit-animation: fadeUp 0.8s ease forwards; animation: fadeUp 0.8s ease forwards; }
.daily-card:hover {
  background-color: rgba(232,132,90,0.06);
  border-color: rgba(232,132,90,0.22);
  -webkit-transform: translateY(-4px);
  transform: translateY(-4px);
}
.daily-card::after {
  content: '';
  position: absolute;
  bottom: 0; left: 0;
  width: 0; height: 1px;
  background-color: #E8845A;
  -webkit-transition: width 0.5s ease;
  transition: width 0.5s ease;
}
.daily-card:hover::after { width: 100%; }

.card-icon { font-size: 1.5rem; margin-bottom: 1.2rem; display: block; }
.card-label { font-size: 0.6rem; letter-spacing: 0.4em; text-transform: uppercase; color: #D4A96A; display: block; margin-bottom: 0.8rem; }
.card-title { font-family: Georgia, serif; font-size: 1.2rem; font-style: italic; color: #FAF0E6; margin-bottom: 0.8rem; }
.card-text { font-size: 0.9rem; color: #E8D8C8; line-height: 1.7; }
.card-hover-msg {
  position: absolute;
  bottom: 1.2rem; left: 2rem; right: 2rem;
  font-size: 0.75rem;
  font-style: italic;
  color: #E8845A;
  opacity: 0;
  -webkit-transform: translateY(5px);
  transform: translateY(5px);
  -webkit-transition: opacity 0.3s ease, -webkit-transform 0.3s ease;
  transition: opacity 0.3s ease, transform 0.3s ease;
  letter-spacing: 0.05em;
}
.daily-card:hover .card-hover-msg { opacity: 1; -webkit-transform: translateY(0); transform: translateY(0); }

/* CITATION */
.quote-section { padding: 8rem 2rem; text-align: center; max-width: 700px; margin: 0 auto; }
.quote-mark { font-family: Georgia, serif; fo