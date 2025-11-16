/*
Next.js Portfolio (Black & White theme) - Scaffold
Files included below separated by comments like: === file: path ===

How to use:
1. Create a new Next.js app: npx create-next-app@latest my-portfolio
2. Replace / add the following files accordingly.
3. Install Tailwind: follow Tailwind + Next.js setup or run: npm install -D tailwindcss postcss autoprefixer && npx tailwindcss init -p
4. npm install
5. npm run dev

This scaffold uses Tailwind CSS for styles and includes:
- Dark/Light toggle
- Black & White minimalist banner (SVG)
- Animated stats section
- ResumeHeader component for PDF-ready header (HTML/CSS)

*/

/* === file: package.json === */
{
  "name": "nextjs-portfolio-blackwhite",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "next": "13.5.6",
    "react": "18.2.0",
    "react-dom": "18.2.0"
  },
  "devDependencies": {
    "autoprefixer": "10.4.14",
    "postcss": "8.4.24",
    "tailwindcss": "3.4.7"
  }
}

/* === file: tailwind.config.js === */
module.exports = {
  content: ["./pages/**/*.{js,jsx}", "./components/**/*.{js,jsx}"],
  darkMode: 'class',
  theme: {
    extend: {
      colors: {
        bwbg: '#0b0b0b',
        bwfg: '#ffffff'
      }
    }
  },
  plugins: []
}

/* === file: postcss.config.js === */
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {}
  }
}

/* === file: styles/globals.css === */
@tailwind base;
@tailwind components;
@tailwind utilities;

:root {
  --bg: #ffffff;
  --fg: #0b0b0b;
}

.dark {
  --bg: #0b0b0b;
  --fg: #ffffff;
}

body {
  background-color: var(--bg);
  color: var(--fg);
  transition: background-color 250ms ease, color 250ms ease;
}

/* minimal card */
.card {
  background: transparent;
  border: 1px solid rgba(0,0,0,0.08);
  padding: 1rem;
  border-radius: 12px;
}

/* resume header printable */
.resume-header {
  display: flex;
  flex-direction: column;
  gap: 6px;
  padding: 0.8rem;
}

/* small utilities */
.center { display:flex; align-items:center; justify-content:center; }

/* === file: pages/_app.js === */
import '../styles/globals.css'
import { useEffect, useState } from 'react'

export default function App({ Component, pageProps }) {
  const [theme, setTheme] = useState('light')
  useEffect(() => {
    const t = localStorage.getItem('theme') || 'light'
    setTheme(t)
    if (t === 'dark') document.documentElement.classList.add('dark')
    else document.documentElement.classList.remove('dark')
  }, [])
  const toggle = () => {
    const next = theme === 'dark' ? 'light' : 'dark'
    setTheme(next)
    if (next === 'dark') document.documentElement.classList.add('dark')
    else document.documentElement.classList.remove('dark')
    localStorage.setItem('theme', next)
  }
  return (
    <div className="min-h-screen">
      <Component {...pageProps} theme={theme} toggleTheme={toggle} />
    </div>
  )
}

/* === file: components/Navbar.jsx === */
export default function Navbar({ toggleTheme, theme }){
  return (
    <nav className="w-full py-4 px-6 flex items-center justify-between">
      <div className="flex items-center gap-3">
        <svg width="36" height="36" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg" aria-hidden>
          <rect x="6" y="6" width="88" height="88" rx="14" fill="none" stroke="currentColor" strokeWidth="6" />
          <text x="50" y="60" fontSize="36" textAnchor="middle" fill="currentColor">OJ</text>
        </svg>
        <div className="text-sm">
          <div className="font-bold">Ojasvee Gupta</div>
          <div className="text-xs opacity-70">Software Developer</div>
        </div>
      </div>
      <div className="flex items-center gap-3">
        <button onClick={toggleTheme} className="px-3 py-1 rounded-md border">
          {theme === 'dark' ? 'Light' : 'Dark'}
        </button>
      </div>
    </nav>
  )
}

/* === file: components/Banner.jsx === */
export default function Banner(){
  return (
    <header className="py-12 px-6 center flex-col gap-4">
      <div className="max-w-3xl text-center">
        <h1 className="text-4xl font-extrabold">Ojasvee Gupta</h1>
        <p className="mt-3 text-lg opacity-80">SDE • Full-Stack Developer • ML Enthusiast</p>
      </div>

      <div className="mt-6">
        {/* minimal black-white SVG banner */}
        <svg width="820" height="120" viewBox="0 0 820 120" xmlns="http://www.w3.org/2000/svg" role="img">
          <rect width="820" height="120" rx="12" fill="none" stroke="currentColor" strokeWidth="1" />
          <g transform="translate(24,24)" fill="none" stroke="currentColor">
            <path d="M0 60 Q120 0 240 60 T480 60 T720 60" strokeWidth="1" opacity="0.7" />
          </g>
        </svg>
      </div>
    </header>
  )
}

/* === file: components/Stats.jsx === */
import { useEffect, useState } from 'react'

function StatCard({label, value}){
  return (
    <div className="card shadow-sm p-4 w-full max-w-xs">
      <div className="text-xs opacity-70">{label}</div>
      <div className="text-2xl font-bold mt-2">{value}</div>
    </div>
  )
}

export default function Stats(){
  // placeholder values - replace with real APIs (github-readme-stats or GH REST API)
  const [stats, setStats] = useState({repos: 24, stars: 142, solved: 550})
  useEffect(()=>{
    // Example: fetch GitHub stats or LeetCode stats here
  },[])
  return (
    <section className="py-8 px-6 center flex-col gap-6">
      <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
        <StatCard label="Public Repos" value={stats.repos} />
        <StatCard label="GitHub Stars" value={stats.stars} />
        <StatCard label="DSA Problems" value={stats.solved} />
      </div>
    </section>
  )
}

/* === file: components/ResumeHeader.jsx === */
export default function ResumeHeader(){
  return (
    <div className="resume-header card max-w-2xl mx-auto">
      <div style={{display:'flex', justifyContent:'space-between', alignItems:'center'}}>
        <div>
          <h2 className="text-2xl font-bold">Ojasvee Gupta</h2>
          <div className="text-sm opacity-80">Software Developer • Full-Stack • ML</div>
        </div>
        <div className="text-right text-sm opacity-80">
          <div>📍 India</div>
          <div>✉️ ojasveegupta10@gmail.com</div>
          <div>🔗 linkedin.com/in/ojasvee-gupta-830952255</div>
        </div>
      </div>
    </div>
  )
}

/* === file: pages/index.js === */
import Navbar from '../components/Navbar'
import Banner from '../components/Banner'
import Stats from '../components/Stats'
import ResumeHeader from '../components/ResumeHeader'

export default function Home({ theme, toggleTheme }){
  return (
    <main className="min-h-screen">
      <Navbar toggleTheme={toggleTheme} theme={theme} />
      <Banner />
      <Stats />

      <section className="py-8 px-6">
        <div className="max-w-4xl mx-auto">
          <h3 className="text-xl font-semibold">About</h3>
          <p className="mt-3 opacity-80">I build full-stack applications and experiment with ML models. My focus is efficient design and production-ready systems. Experienced in C++, Python, React, Node.js, MongoDB and cloud services.</p>
        </div>
      </section>

      <section className="py-8 px-6 bg-transparent">
        <div className="max-w-4xl mx-auto">
          <h3 className="text-xl font-semibold">Projects</h3>
          <ul className="mt-4 space-y-3">
            <li className="card p-4">Project A — Short description. <a className="underline" href="#">Repo</a></li>
            <li className="card p-4">Project B — Short description. <a className="underline" href="#">Repo</a></li>
          </ul>
        </div>
      </section>

      <section className="py-8 px-6">
        <div className="max-w-4xl mx-auto">
          <h3 className="text-xl font-semibold">Resume Header (Preview)</h3>
          <div className="mt-4"><ResumeHeader /></div>
        </div>
      </section>

      <footer className="py-8 px-6 center">
        <div className="opacity-70">© {new Date().getFullYear()} Ojasvee Gupta — Designed with a minimalist black & white theme.</div>
      </footer>
    </main>
  )
}

/* === file: public/banner.svg === */
<!-- Minimalist black-white banner (optional) -->
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 200">
  <rect width="1200" height="200" rx="12" fill="none" stroke="black" opacity="0.06"/>
  <text x="50%" y="50%" dominant-baseline="middle" text-anchor="middle" font-size="36">Ojasvee • Software Developer</text>
</svg>

/* === file: README.md (project usage) === */
# Next.js Portfolio - Black & White

This repository contains a minimal Next.js portfolio scaffold with a black & white theme, dark/light toggle, and components for a banner, animated stats, and a printable resume header.

## Setup
1. Install dependencies: `npm install`
2. Run dev server: `npm run dev`

## Notes
- Replace placeholder data with your real project links and API calls (GitHub/LeetCode) if desired.
- To export a PDF resume: open the ResumeHeader route and print to PDF using the browser's print dialog.


/* End of scaffold */
