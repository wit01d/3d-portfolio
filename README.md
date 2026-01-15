# 3D Portfolio

![React](https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=white)
![Three.js](https://img.shields.io/badge/Three.js-0.174-000000?style=for-the-badge&logo=threedotjs&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-6-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind-4-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)
![GSAP](https://img.shields.io/badge/GSAP-3.12-88CE02?style=for-the-badge&logo=greensock&logoColor=white)

An immersive 3D portfolio experience built with React Three Fiber, GSAP animations, and modern web technologies. This project transforms a traditional developer portfolio into an interactive showcase featuring real-time 3D graphics, particle effects, and scroll-triggered animations.

---

## Table of Contents

- [Demo](#demo)
- [The Story](#the-story)
  - [Challenge](#challenge)
  - [Journey](#journey)
  - [Discovery](#discovery)
  - [Innovation](#innovation)
  - [Breakthrough](#breakthrough)
  - [Transformation](#transformation)
  - [Impact](#impact)
  - [Collaboration](#collaboration)
  - [Lessons Learned](#lessons-learned)
  - [Future](#future)
- [Features](#features)
- [Screenshots](#screenshots)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Scripts](#scripts)
- [Configuration](#configuration)
- [Contributing](#contributing)
- [License](#license)

---

## Demo

![3D Portfolio Demo](./docs/demo.gif)

*Interactive 3D hero section with particle effects and draggable camera controls*

> **Note**: Replace `./docs/demo.gif` with your own screen recording.
> - Recommended tool: [ScreenToGif](https://www.screentogif.com/) or [LICEcap](https://www.cockos.com/licecap/)
> - Optimal size: 800x450px, under 5MB

---

## The Story

### Challenge

**The Problem**: Traditional developer portfolios fail to stand out.

Building a portfolio that truly demonstrates technical expertise presented several obstacles:

- **Static limitations**: Traditional HTML/CSS portfolios lack the interactivity needed to showcase modern web development skills
- **First impressions matter**: In a crowded field of developer portfolios, standing out requires more than just listing projects
- **Performance concerns**: Integrating 3D graphics on the web without sacrificing load times and responsiveness seemed daunting
- **Device compatibility**: Ensuring the experience works across desktop, tablet, and mobile devices

> "We faced the challenge of creating a portfolio that would not only display work but actively demonstrate the technical capabilities it claims to represent."

---

### Journey

**The Process**: From concept to immersive experience.

The development journey followed a structured path through key milestones:

```
Phase 1: Foundation
├── React 19 + Vite 6 setup
├── Tailwind CSS 4 integration
└── Component architecture planning

Phase 2: 3D Integration
├── React Three Fiber implementation
├── GLB model optimization
└── Lighting and camera setup

Phase 3: Animation Layer
├── GSAP timeline animations
├── ScrollTrigger integration
└── Staggered text reveals

Phase 4: Polish & Optimization
├── Responsive 3D scaling
├── Model preloading
└── Performance tuning
```

The application architecture evolved into a clean, section-based structure:

```jsx
// App.jsx - The composition of sections
const App = () => (
  <>
    <Navbar />
    <Hero />
    <ShowcaseSection />
    <LogoShowcase />
    <FeatureCards />
    <Experience />
    <TechStack />
    <Testimonials />
    <Contact />
    <Footer />
  </>
);
```

---

### Discovery

**Key Insights**: Performance optimization breakthroughs.

Several critical discoveries shaped the project's success:

#### Model Preloading Strategy

We discovered that preloading 3D models prevents loading jank and creates a seamless user experience:

```jsx
import { useGLTF } from "@react-three/drei";

// Preload models at module level
useGLTF.preload("/models/room-transformed.glb");
useGLTF.preload("/models/computer-optimized-transformed.glb");
```

#### Responsive 3D Scaling

Finding the right approach to responsive 3D was crucial. Device-specific scaling ensures the 3D elements remain impactful without overwhelming smaller screens:

```jsx
const isMobile = useMediaQuery({ query: "(max-width: 768px)" });

<group
  scale={isMobile ? 0.7 : 1}
  position={[0, -3.5, 0]}
>
  <Room />
</group>
```

#### Suspense Boundaries

Using React's Suspense prevents layout shift during 3D asset loading, maintaining a professional feel:

```jsx
<Canvas camera={{ position: [0, 0, 15], fov: 45 }}>
  <Suspense fallback={null}>
    <HeroLights />
    <Particles count={100} />
    <Room />
  </Suspense>
</Canvas>
```

---

### Innovation

**Creative Solutions**: Novel approaches that set this project apart.

#### Custom Particle System

A falling particle effect adds ambient atmosphere to the hero section, built from scratch using Three.js primitives:

```jsx
const Particles = ({ count = 200 }) => {
  const mesh = useRef();

  const particles = useMemo(() => {
    const temp = [];
    for (let i = 0; i < count; i++) {
      temp.push({
        position: [
          (Math.random() - 0.5) * 10,
          Math.random() * 10 + 5,
          (Math.random() - 0.5) * 10,
        ],
        speed: 0.005 + Math.random() * 0.001,
      });
    }
    return temp;
  }, [count]);

  useFrame(() => {
    const positions = mesh.current.geometry.attributes.position.array;
    for (let i = 0; i < count; i++) {
      let y = positions[i * 3 + 1];
      y -= particles[i].speed;
      if (y < -2) y = Math.random() * 10 + 5;
      positions[i * 3 + 1] = y;
    }
    mesh.current.geometry.attributes.position.needsUpdate = true;
  });

  return (
    <points ref={mesh}>
      <bufferGeometry>
        <bufferAttribute
          attach="attributes-position"
          count={count}
          array={positions}
          itemSize={3}
        />
      </bufferGeometry>
      <pointsMaterial
        color="#ffffff"
        size={0.05}
        transparent
        opacity={0.9}
      />
    </points>
  );
};
```

#### Interactive GlowCard Effect

Cards with dynamic glow effects that follow mouse movement, calculated using trigonometry:

```jsx
const handleMouseMove = (index) => (e) => {
  const card = cardRefs.current[index];
  const rect = card.getBoundingClientRect();
  const mouseX = e.clientX - rect.left - rect.width / 2;
  const mouseY = e.clientY - rect.top - rect.height / 2;

  // Calculate angle from center to mouse
  let angle = Math.atan2(mouseY, mouseX) * (180 / Math.PI);
  angle = (angle + 360) % 360;

  // Apply as CSS custom property
  card.style.setProperty("--start", angle + 60);
};
```

#### Serverless Email Integration

Contact form powered by EmailJS, eliminating the need for backend infrastructure while maintaining professional functionality.

---

### Breakthrough

**Pivotal Moments**: Technical wins that unlocked the vision.

#### OrbitControls Configuration

The breakthrough in user interaction came from carefully tuning OrbitControls for an intuitive yet constrained 3D experience:

```jsx
<OrbitControls
  enablePan={false}           // Prevent accidental scene panning
  enableZoom={!isTablet}      // Disable zoom on touch devices
  maxDistance={20}            // Limit zoom out
  minDistance={5}             // Limit zoom in
  minPolarAngle={Math.PI / 5} // Prevent looking from below
  maxPolarAngle={Math.PI / 2} // Prevent looking from above
/>
```

#### Canvas + Suspense Pattern

Combining React Three Fiber's Canvas with Suspense boundaries created the smooth loading experience users expect:

```jsx
<Canvas camera={{ position: [0, 0, 15], fov: 45 }}>
  <ambientLight intensity={0.2} color="#1a1a40" />
  <OrbitControls {...controlsConfig} />

  <Suspense fallback={null}>
    <HeroLights />
    <Particles count={100} />
    <group scale={isMobile ? 0.7 : 1}>
      <Room />
    </group>
  </Suspense>
</Canvas>
```

#### GSAP ScrollTrigger Integration

Scroll-driven animations bring the experience to life as users navigate through sections, creating a narrative flow.

---

### Transformation

**Before & After**: The evolution from concept to reality.

| Aspect | Before | After |
|--------|--------|-------|
| **Visual Impact** | Static HTML/CSS layout | Immersive 3D environment with particle effects |
| **User Engagement** | Passive scrolling | Interactive camera controls and animations |
| **Loading Experience** | Flash of unstyled content | Smooth, progressive asset loading |
| **Mobile Experience** | Often broken 3D | Responsive scaling with touch-optimized controls |
| **First Impression** | "Another portfolio" | "This developer knows their craft" |

The transformation extended beyond visuals:

- **Performance**: From page-load jank to smooth 60fps animations
- **Architecture**: From monolithic code to modular, reusable components
- **Maintainability**: From scattered data to centralized constants

---

### Impact

**Results & Benefits**: The tangible outcomes of this approach.

#### For Visitors
- **Memorable first impression** that distinguishes from typical portfolios
- **Interactive exploration** that invites engagement rather than passive viewing
- **Seamless experience** across all devices and screen sizes

#### For Developers
- **Demonstrates expertise** in modern web technologies without needing to explain it
- **Showcases full-stack capabilities** from 3D graphics to backend email integration
- **Provides a template** that can be adapted and extended for personal use

#### For the Community
- **Open source reference** for React Three Fiber implementations
- **Pattern library** for GSAP + React integration
- **Performance benchmarks** for 3D on the web

---

### Collaboration

**Architecture for Teamwork**: Structure that enables contribution.

The project's architecture was designed with collaboration in mind:

```
src/
├── components/           # Reusable UI components
│   ├── Button.jsx        # CTA buttons
│   ├── GlowCard.jsx      # Interactive card with effects
│   ├── NavBar.jsx        # Navigation component
│   ├── TitleHeader.jsx   # Section headers
│   └── AnimatedCounter.jsx
│
├── components/models/    # 3D experience components
│   ├── hero_models/      # Hero section 3D
│   ├── contact/          # Contact section 3D
│   └── tech_logos/       # Tech stack 3D icons
│
├── sections/             # Full-page sections
│   ├── Hero.jsx
│   ├── Experience.jsx
│   ├── TechStack.jsx
│   └── Contact.jsx
│
└── constants/            # Centralized data
    └── index.js          # All static content
```

#### Key Collaboration Patterns

- **Separation of concerns**: 3D logic lives in `/models`, UI in `/components`, page layout in `/sections`
- **Centralized data**: All static content in `/constants/index.js` for easy updates
- **Reusable components**: Button, GlowCard, TitleHeader can be used across sections

---

### Lessons Learned

**Key Takeaways**: Insights for future projects.

#### Technical Insights

1. **GLB is the optimal format** for web 3D models - compressed, fast to load, and widely supported
2. **Device-specific rendering is essential** - what works on desktop may overwhelm mobile
3. **Suspense boundaries prevent layout shift** - always wrap async 3D content
4. **CSS custom properties enable dynamic effects** - the GlowCard's angle tracking uses `--start`

#### Architecture Decisions

1. **Centralize configuration** - keeping all data in `/constants/index.js` makes updates trivial
2. **Component composition over inheritance** - React patterns shine with 3D integration
3. **Preload critical assets** - `useGLTF.preload()` at module level prevents jank

#### Performance Considerations

1. **Limit particle counts** on mobile devices
2. **Disable expensive features** (zoom, complex controls) on tablets
3. **Use `depthWrite={false}`** for transparent materials to avoid z-fighting

---

### Future

**What's Next**: Roadmap and opportunities.

#### Planned Enhancements

- [ ] **AI Integration** - Infrastructure for GEMINI_API_KEY is ready for chatbot features
- [ ] **Additional 3D Interactions** - More interactive models in project showcases
- [ ] **Performance Analytics** - Monitoring and optimization based on real user data
- [ ] **Accessibility Improvements** - Enhanced screen reader support for 3D content

#### Community Opportunities

- **Theming system** for easy customization
- **Plugin architecture** for additional sections
- **Animation presets** for common use cases
- **Documentation site** with interactive examples

---

## Features

| Feature | Description |
|---------|-------------|
| **3D Hero Section** | Interactive room model with OrbitControls and particle effects |
| **Experience Timeline** | GSAP ScrollTrigger animated career history |
| **Tech Stack Cards** | Five interactive 3D cards showcasing technologies |
| **GlowCard Effects** | Mouse-tracking gradient glow on testimonial cards |
| **Animated Counters** | Statistics that count up on scroll |
| **Contact Form** | EmailJS integration with loading states |
| **Responsive Design** | Optimized for desktop, tablet, and mobile |
| **Logo Carousel** | Client/partner logo showcase |

---

## Screenshots

| Hero Section | Experience Timeline | Tech Stack |
|:---:|:---:|:---:|
| ![Hero Section with 3D room model](./docs/hero.png) | ![Experience timeline with animations](./docs/experience.png) | ![Interactive tech stack cards](./docs/tech.png) |

| Contact Form | Testimonials | Mobile View |
|:---:|:---:|:---:|
| ![Contact form with 3D computer](./docs/contact.png) | ![Testimonials with glow cards](./docs/testimonials.png) | ![Mobile responsive view](./docs/mobile.png) |

> **Note**: Create a `docs/` folder and add your screenshots. Recommended size: 400x300px per image.

---

## Getting Started

### Prerequisites

- Node.js 18+
- npm or yarn

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/3d-portfolio.git

# Navigate to project directory
cd 3d-portfolio

# Install dependencies
npm install
```

### Environment Setup

Create a `.env` file in the root directory:

```env
# EmailJS Configuration (for contact form)
VITE_EMAILJS_SERVICE_ID=your_service_id
VITE_EMAILJS_TEMPLATE_ID=your_template_id
VITE_EMAILJS_PUBLIC_KEY=your_public_key
```

> Get your EmailJS credentials at [emailjs.com](https://www.emailjs.com/)

### Development

```bash
# Start development server
npm run dev

# Server runs at http://localhost:3000
```

---

## Project Structure

```
3d-portfolio/
├── public/
│   ├── models/              # GLB 3D models
│   │   ├── room-transformed.glb
│   │   ├── computer-optimized-transformed.glb
│   │   └── tech logos...
│   └── images/              # Static images
│
├── src/
│   ├── components/
│   │   ├── models/          # 3D React Three Fiber components
│   │   │   ├── hero_models/
│   │   │   │   ├── Room.jsx
│   │   │   │   ├── HeroLights.jsx
│   │   │   │   ├── Particles.jsx
│   │   │   │   └── HeroExperience.jsx
│   │   │   ├── contact/
│   │   │   │   ├── Computer.jsx
│   │   │   │   └── ContactExperience.jsx
│   │   │   └── tech_logos/
│   │   │       └── TechIconCardExperience.jsx
│   │   │
│   │   ├── AnimatedCounter.jsx
│   │   ├── Button.jsx
│   │   ├── GlowCard.jsx
│   │   ├── NavBar.jsx
│   │   └── TitleHeader.jsx
│   │
│   ├── sections/
│   │   ├── Hero.jsx
│   │   ├── ShowcaseSection.jsx
│   │   ├── LogoShowcase.jsx
│   │   ├── FeatureCards.jsx
│   │   ├── Experience.jsx
│   │   ├── TechStack.jsx
│   │   ├── Testimonials.jsx
│   │   ├── Contact.jsx
│   │   └── Footer.jsx
│   │
│   ├── constants/
│   │   └── index.js         # All static data
│   │
│   ├── App.jsx
│   ├── main.jsx
│   └── index.css
│
├── .env.example
├── vite.config.js
├── tailwind.config.js
└── package.json
```

---

## Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start development server on port 3000 |
| `npm run build` | Create production build |
| `npm run preview` | Preview production build locally |
| `npm run lint` | Run ESLint for code quality |

---

## Configuration

### Vite Configuration

```javascript
// vite.config.js
export default defineConfig({
  plugins: [react()],
  server: {
    port: 3000,
    allowedHosts: ['yourdomain.com']
  }
});
```

### Tailwind CSS

This project uses Tailwind CSS v4 with the Vite plugin:

```javascript
// tailwind.config.js
export default {
  content: ["./index.html", "./src/**/*.{js,jsx}"],
  theme: {
    extend: {
      // Custom theme extensions
    }
  }
};
```

### EmailJS Integration

The contact form uses EmailJS for serverless email delivery:

```jsx
import emailjs from "@emailjs/browser";

emailjs.send(
  import.meta.env.VITE_EMAILJS_SERVICE_ID,
  import.meta.env.VITE_EMAILJS_TEMPLATE_ID,
  formData,
  import.meta.env.VITE_EMAILJS_PUBLIC_KEY
);
```

---

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Follow the existing component structure
- Add new static data to `/constants/index.js`
- Optimize 3D models to GLB format before adding
- Test on mobile devices before submitting

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- [React Three Fiber](https://docs.pmnd.rs/react-three-fiber) for making 3D in React accessible
- [GSAP](https://greensock.com/gsap/) for powerful animation capabilities
- [Drei](https://github.com/pmndrs/drei) for useful Three.js abstractions
- [EmailJS](https://www.emailjs.com/) for serverless email functionality

---

<p align="center">
  Built with React Three Fiber and GSAP
</p>
