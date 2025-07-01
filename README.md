# Astro Project with Lenis and GSAP

This project demonstrates how to create an Astro project and integrate the Lenis smooth scrolling library and GSAP animation framework.

## Getting Started

Follow the steps below to set up the project:

### 1. Create a New Astro Project

1. Install the Astro CLI if you haven't already:
   ```sh
   npm create astro@latest
   ```

2. Follow the prompts to set up your project. Choose a starter template or start from scratch.

3. Navigate to your project directory:
   ```sh
   cd your-project-name
   ```

4. Install dependencies:
   ```sh
   npm install
   ```

### 2. Install Lenis and GSAP

1. Add Lenis and GSAP to your project:
   ```sh
   npm install @studio-freight/lenis gsap
   ```

### 3. Add Lenis and GSAP Example

1. Create a layout file `src/layouts/Layout.astro`:
   ```astro
   ---
   export interface Props {
     title: string;
   }

   const { title } = Astro.props;
   ---

   <!DOCTYPE html>
   <html lang="en">
     <head>
       <meta charset="UTF-8" />
       <meta name="description" content="Astro description">
       <meta name="viewport" content="width=device-width" />
       <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
       <title>{title}</title>
     </head>
     <body>
       <slot />
     </body>
   </html>

   <style is:global>
     * {
       margin: 0;
       padding: 0;
       box-sizing: border-box;
     }

     body {
       font-family: Arial, sans-serif;
     }
   </style>
   ```

2. Create the main page `src/pages/index.astro`:
   ```astro
   ---
   import Layout from '../layouts/Layout.astro';
   ---

   <Layout title="Lenis & GSAP Demo">
     <main>
       <section class="hero">
         <h1 class="title">Smooth Scrolling with Lenis</h1>
         <p class="subtitle">Scroll down to see GSAP animations</p>
       </section>

       <section class="content">
         <div class="box box1">Box 1</div>
         <div class="box box2">Box 2</div>
         <div class="box box3">Box 3</div>
       </section>

       <section class="footer">
         <h2>End of Demo</h2>
         <p>Lenis provides smooth scrolling, GSAP handles animations</p>
       </section>
     </main>
   </Layout>

   <style>
     .hero {
       height: 100vh;
       display: flex;
       flex-direction: column;
       justify-content: center;
       align-items: center;
       background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
       color: white;
       text-align: center;
     }

     .title {
       font-size: 3rem;
       margin-bottom: 1rem;
       opacity: 0;
       transform: translateY(50px);
     }

     .subtitle {
       font-size: 1.2rem;
       opacity: 0;
       transform: translateY(30px);
     }

     .content {
       min-height: 200vh;
       padding: 4rem 2rem;
       display: flex;
       flex-direction: column;
       gap: 4rem;
       align-items: center;
       background: #f5f5f5;
     }

     .box {
       width: 200px;
       height: 200px;
       border-radius: 20px;
       display: flex;
       align-items: center;
       justify-content: center;
       color: white;
       font-size: 1.5rem;
       font-weight: bold;
       transform: translateX(-100px);
       opacity: 0;
     }

     .box1 { background: #ff6b6b; }
     .box2 { background: #4ecdc4; }
     .box3 { background: #45b7d1; }

     .footer {
       height: 100vh;
       display: flex;
       flex-direction: column;
       justify-content: center;
       align-items: center;
       background: #2c3e50;
       color: white;
       text-align: center;
     }

     .footer h2 {
       font-size: 2.5rem;
       margin-bottom: 1rem;
     }
   </style>

   <script>
     import Lenis from '@studio-freight/lenis'
     import { gsap } from 'gsap'
     import { ScrollTrigger } from 'gsap/ScrollTrigger'

     // Register ScrollTrigger plugin
     gsap.registerPlugin(ScrollTrigger);

     // Initialize Lenis
     const lenis = new Lenis({
       duration: 1.2,
       easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
       smooth: true,
     });

     function raf(time) {
       lenis.raf(time);
       requestAnimationFrame(raf);
     }

     requestAnimationFrame(raf);

     // GSAP Animations
     // Hero animations
     gsap.to('.title', {
       opacity: 1,
       y: 0,
       duration: 1,
       delay: 0.5,
       ease: 'power2.out'
     });

     gsap.to('.subtitle', {
       opacity: 1,
       y: 0,
       duration: 1,
       delay: 0.8,
       ease: 'power2.out'
     });

     // Box animations with ScrollTrigger
     gsap.utils.toArray('.box').forEach((box, index) => {
       gsap.to(box, {
         opacity: 1,
         x: 0,
         duration: 1,
         ease: 'power2.out',
         scrollTrigger: {
           trigger: box,
           start: 'top 80%',
           end: 'bottom 20%',
           toggleActions: 'play none none reverse',
         }
       });
     });

     // Update ScrollTrigger on Lenis scroll
     lenis.on('scroll', ScrollTrigger.update);
   </script>
   ```

### 4. Run the Project

Start the development server:
```sh
npm run dev
```

Visit `http://localhost:4321` in your browser to see the Lenis and GSAP example in action.

## Features Demonstrated

- **Lenis Smooth Scrolling**: Provides buttery smooth scrolling experience
- **GSAP Animations**: Hero text animations and scroll-triggered box animations
- **ScrollTrigger Integration**: Animations triggered by scroll position
- **Responsive Design**: Works on different screen sizes

## Project Structure

```
src/
├── layouts/
│   └── Layout.astro          # Base layout component
├── pages/
│   └── index.astro           # Main page with Lenis & GSAP demo
└── ...
```

## Learn More

- [Astro Documentation](https://docs.astro.build/)
- [Lenis Documentation](https://github.com/studio-freight/lenis)
- [GSAP Documentation](https://greensock.com/gsap/)
- [ScrollTrigger Documentation](https://greensock.com/scrolltrigger/)
