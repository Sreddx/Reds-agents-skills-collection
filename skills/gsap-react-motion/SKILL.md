---
name: gsap-react-motion
description: Use when a frontend task needs GSAP in React or Next.js for timelines, ScrollTrigger, SVG morphing, kinetic typography, parallax, or advanced interaction motion. Optimized for production-safe integration with `@gsap/react`, cleanup, SSR awareness, reduced-motion support, and performance discipline.
---

## Purpose

Provide concrete implementation guidance for production-safe GSAP usage in React ecosystems.

Use this skill when the work needs:
- multi-step timelines
- ScrollTrigger or scroll storytelling
- advanced hero animations
- SVG morphing or kinetic typography
- richer motion systems than CSS-only transitions can comfortably support

Do not use this skill for trivial hover/fade work that CSS or the existing component system can solve more simply.

## Core rules

- In React, prefer `@gsap/react` and `useGSAP()`
- Register the hook/plugin explicitly: `gsap.registerPlugin(useGSAP)`
- Scope selector text with a container ref
- Ensure cleanup through GSAP context reversion
- For event handlers or delayed callbacks, wrap with `contextSafe()`
- In SSR environments like Next.js App Router, use GSAP only in client components
- Respect `prefers-reduced-motion`
- Prefer animating `transform` and `opacity`
- Avoid layout-thrashing properties unless there is a strong reason

## Default implementation pattern

```tsx
'use client'

import { useRef } from 'react'
import gsap from 'gsap'
import { useGSAP } from '@gsap/react'

gsap.registerPlugin(useGSAP)

export function HeroMotion() {
  const container = useRef<HTMLDivElement | null>(null)

  useGSAP(() => {
    gsap.from('.hero-item', {
      y: 24,
      opacity: 0,
      duration: 0.6,
      stagger: 0.08,
      ease: 'power2.out',
    })
  }, { scope: container })

  return (
    <section ref={container}>
      <h1 className="hero-item">Title</h1>
      <p className="hero-item">Copy</p>
      <button className="hero-item">CTA</button>
    </section>
  )
}
```

## ScrollTrigger guidance

Use ScrollTrigger when:
- the page has narrative sections
- scroll position drives reveal/progress states
- there is clear product or storytelling value

Avoid it when:
- the page is form-heavy
- the interface is primarily operational/data-dense
- accessibility or motion sensitivity makes the effect risky
- mobile performance budget is tight and no fallback exists

## Interaction safety

If animation is created after the main `useGSAP()` execution, wrap it with `contextSafe()`.

```tsx
const { contextSafe } = useGSAP({ scope: container })

const onClick = contextSafe(() => {
  gsap.to('.cta', { scale: 1.06, duration: 0.2 })
})
```

## Accessibility rules

- Respect `prefers-reduced-motion: reduce`
- Provide non-motion fallback for critical information
- Avoid autoplay motion that interferes with reading or task completion
- Keep text readable during animation
- Do not make essential UI discoverability depend exclusively on motion

## Performance checklist

- animate `transform` and `opacity` first
- use short timelines and kill/revert on teardown
- avoid too many concurrent ScrollTriggers
- test low-power/mobile scenarios for scroll-heavy experiences
- avoid animating large shadows, filters, or layout properties at scale

## Decision heuristic

Use GSAP when at least one of these is true:
- timeline orchestration matters
- scroll-linked motion is a product requirement
- SVG/path morphing is needed
- the motion design is central to the experience

Otherwise, default to CSS transitions or the simpler motion primitive already in the stack.

## Deliverables

When this skill is used, include:
- why GSAP was chosen over simpler motion options
- where `useGSAP()` is mounted
- reduced-motion behavior
- cleanup/scope approach
- performance caveats if ScrollTrigger or heavy motion is used

## Sources

Primary references checked:
- https://gsap.com/resources/React/
- https://github.com/greensock/react/blob/main/README.md
