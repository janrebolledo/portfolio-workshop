# personal portfolio workshop 🌐

<video controls width="640">
  <source src="https://github.com/janrebolledo/portfolio-workshop/raw/refs/heads/master/demo.mp4" type="video/mp4">
  your browser does not support the video tag.
</video>

intro:
what is a personal portfolio, why build one, project visibility & personal branding, how will we design it, what tools we use (astro, tailwind, markdown)

resources:

- [astro docs](https://docs.astro.build/)
- [tailwind docs](https://tailwindcss.com/docs)
- [workshop slides](https://www.figma.com/deck/ta8dqRu8qSGhYYO88FIgWV/Jan-for-CSS---SEA---Personal-Website?node-id=1-42&t=wuSt3EkUVWGkS5Td-1)
- [figma design](https://www.figma.com/design/JrVxg7qCZgJhausEqRz3xl/Jan-for-CSS---SEA---Personal-Website?node-id=0-1&t=iHVXTtfxG9oQkNbg-1)

prereqs:

- [vscode](https://code.visualstudio.com/download)
- [node + npm](https://nodejs.org/en/download)

post workshop exploration:

- add an about page
- [connect a cms](https://docs.astro.build/en/guides/cms/) for managing content

<details>
<summary>scaffolding the project from scratch</summary>

initialize astro project

```sh
npm create astro@latest -- --template minimal
```

add project dependencies

```sh
npx astro add tailwind mdx
```

```sh
npm install -D @tailwindcss/typography
```

create `layouts` & `projects` folders in `/src`

```sh
mkdir ./src/pages/projects && mkdir ./src/layouts
```

add a web font

```css
/* globals.css */

@import url('https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&display=swap');
@import 'tailwindcss';
@plugin "@tailwindcss/typography";

@theme {
  --font-sans: 'Inter', sans-serif;
}

body {
  font-family: 'Inter', sans-serif;
}

@view-transition {
  navigation: auto;
}
```

</details>

start the development server

```sh
npm run dev
```

## importing projects

```jsx
interface Project {
  url: string;
  frontmatter: {
    title: string;
    type: string;
    image: string;
  };
}

const projects = Object.values(
  import.meta.glob<Project>('./projects/*.md', { eager: true }),
);
```

## creating project cards

```jsx
{
  projects.map((project) => (
    <a
      href={project.url}
      class='flex flex-col gap-2.5 bg-olive-100 rounded-xl p-9 h-[400px] '
    >
      <div class='w-full flex h-full items-center justify-center'>
        <img
          src={project.frontmatter.image}
          alt={project.frontmatter.title}
          class='w-full object-cover rounded-xl shadow-md object-top'
        />
      </div>
      <div class='flex justify-between items-center gap-2.5 text-black/50'>
        <h2 class='font-medium'>{project.frontmatter.title}</h2>
        <p>{project.frontmatter.type}</p>
      </div>
    </a>
  ));
}
```

## defining the project layout

```jsx
---
import Layout from './Layout.astro';
const frontmatter = Astro.props.frontmatter;
---

<Layout title={frontmatter.title}>
  <main class='container mx-auto px-6'>
    <section class='flex flex-col gap-12'>
      <div
        class='w-full h-[400px] flex bg-olive-100 rounded-xl items-center justify-center p-9'
      >
        <img
          class='w-full md:h-full object-cover rounded-xl shadow-md object-top'
          src={frontmatter.image}
          alt={frontmatter.title}
        />
      </div>
      <div class='flex flex-col md:flex-row justify-between items-center gap-4'>
        <div class='flex flex-col gap-6'>
          <h1 class='text-5xl font-medium'>{frontmatter.title}</h1>
          <p>{frontmatter.description}</p>
        </div>
        <a
          href={frontmatter.projecturl}
          target='_blank'
          class='flex justify-center items-center gap-2.5 px-4 py-3 rounded-xl border border-olive-100 bg-olive-600 text-white w-full md:w-auto'
        >
          View Project
        </a>
      </div>
      <div class='grid grid-cols-1 md:grid-cols-3 gap-4'>
        <div>
          <p class='font-medium text-black/50'>Role</p>
          <ul class='flex flex-col'>
            {frontmatter.role.map((role: string) => <li>{role}</li>)}
          </ul>
        </div>
        <div>
          <p class='font-medium text-black/50'>Tools</p>
          <ul class='flex flex-col'>
            {frontmatter.tools.map((tool: string) => <li>{tool}</li>)}
          </ul>
        </div>
        <div>
          <p class='font-medium text-black/50'>Duration</p>
          <p>{frontmatter.duration}</p>
        </div>
      </div>
    </section>
    <section class='py-32 prose mx-auto'>
      <slot />
    </section>
  </main>
</Layout>
```

the home page automatically picks up any `.md` file in the `projects/` folder 🌱

created with 🫶 by jan rebolledo, for software engineering association & computer science society @ california state polytechnic university, pomona
