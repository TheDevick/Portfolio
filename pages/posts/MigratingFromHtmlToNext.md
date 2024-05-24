---
title: Migrating from HTML to NextJs
date: 2024/05/23
description: Here's how I migrated a HTML Website to NextJs
tag: NextJs
author: TheDevick
---

# The Website

The website I migrated from HTML to NextJs was originally created using the [Massively Template](https://html5up.net/massively). This template is both beautiful and simple. Initially, I was writing pure HTML files to publish new posts, hosting everything on GitHub Pages. However, each time I wanted to publish a new post, I had to copy and paste an old post, ensure all details were updated, and then link it on the index page. As a programmer who loves automating tasks, this process was far from ideal.

![](https://miro.medium.com/v2/resize:fit:735/1*6Oyig2ACF-unC3R-CXT8jw.jpeg)

## The Challenge

I love challenges, so I decided to migrate the entire site to NextJs within a single day.

## Recognizing the Template

My first step was to understand the template's internals. The template used jQuery, which initially concerned me about the compatibility with NextJs/ReactJs. Fortunately, it all worked out in the end. The template also included web fonts and SASS/CSS files. Since I had never worked with SASS before, I considered skipping it to keep the project within my one-day challenge.

For the posts, I decided to use Markdown and FrontMatter, allowing me to store metadata (like publication date, title, and description) inside Markdown files.

## Starting the NextJs Project

I initiated the project by running `npx create-next-app@latest`, opting for pages routing without Tailwind since the styling was already provided by the template. I installed the following libraries to facilitate the transition:

- [clsx](https://www.npmjs.com/package/clsx) for concatenating class names
- [date-fns](https://www.npmjs.com/package/date-fns) for date manipulation
- [contentlayer](https://www.npmjs.com/package/contentlayer) and [next-contentlayer](https://www.npmjs.com/package/next-contentlayer) to handle the markdown files.

## The Layout

I imported the necessary assets into the `Layout.tsx` file and started creating layout components such as the header, footer, etc. The initial layout came together smoothly, and the site looked great.
![](/images/Snapshot1.png)

## The Index Page

The index page consists of two sections: a featured post and a listing of all posts. I replaced the featured post with content about solar energy to grab user attention.
![](/images/Snapshot2.png)

## Setting up  and writing the posts

The posts are the main part of the website. I used the contentlayer library for this. Below is my configuration file:

```ts
// contentlayer.config.ts
import { defineDocumentType, makeSource } from "contentlayer/source-files"

export const Post = defineDocumentType(() => ({
  name: 'Post',
  filePathPattern: '**/*.md',
  fields: {
    title: {
      type: 'string',
      required: true
    },
    date: {
      type: 'date',
      required: true
    },
    description: {
      type: 'string',
      required: true
    },
  },
  computedFields: {
    slug: { type: 'string', resolve: (post) => `/posts/${post._raw.flattenedPath}` },
    mainPicture: { type: 'string', resolve: (post) => `/posts/${post._raw.flattenedPath}/main.jpg` },
    sidePicture: { type: 'string', resolve: (post) => `/posts/${post._raw.flattenedPath}/side.jpg` },
  },
}))

export default makeSource({ contentDirPath: 'src/posts', documentTypes: [Post] })
```

This configuration file defines how the markdown FrontMatter should behave and automatically generates types, which is incredibly useful.

From now on, I'll be giving examples about a post of the [Solar Project Recanto da Amizade](https://www.bilhalba.com.br/posts/recanto-da-amizade).

For each post, I added images in the following structure:

- `public/posts/recanto-da-amizade/main.jpg` Main picture (typically horizontal).
- `public/posts/recanto-da-amizade/side.jpg` Side picture (typically vertical).

I also created folders within public/posts/recanto-da-amizade for additional images, which are displayed as a gallery at the end of each post.

Here is an example of a markdown file:

```md
// src/posts/recanto-da-amizade.html 
---
date: 2024-05-20
title: Bilhalba Engenharia com Projeto Solar no Bar Recanto da Amizade
description: Visando o fortalecimento ambiental e resultados econômicos significativos a longo prazo, o Bar Recanto da Amizade adere a sustentabilidade da Geração Solar.
---

No Bar Recanto da Amizade, foi instalado um gerador solar, composto por painéis solares JinkoSolar de 585W cada e um inversor Growatt, garantindo um sistema eficiente e robusto.

Estas placas são reconhecidas pela durabilidade e excelente desempenho, mesmo em condições de baixa luminosidade, o que é essencial para maximizar a produção de energia solar ao longo do dia. A instalação das 13 placas solares no Bar Recanto da Amizade não só vai permitir a geração de uma quantidade significativa de energia limpa, como também promoverá uma redução considerável nas despesas com eletricidade.

O inversor Growatt de 6 kW foi selecionado pela sua confiabilidade e alta eficiência. Este componente é fundamental para converter a energia gerada pelas placas solares em eletricidade utilizável pelo estabelecimento. Além disso, o monitoramento da geração pode estar na palma de sua mão, através do sistema de monitoramento remoto, permitindo que o cliente e nossa equipe técnica acompanhe em tempo real o desempenho do sistema, identifique possíveis problemas e tome medidas corretivas de maneira rápida e eficiente.

Se você deseja saber mais sobre como um sistema de energia solar pode beneficiar o seu negócio ou residência, nossa equipe está à disposição para fornecer informações detalhadas, realizar orçamentos personalizados e esclarecer todas as suas dúvidas. Entre em contato conosco hoje mesmo e descubra como podemos ajudar você a adotar uma solução energética sustentável e eficiente.
```

### Creating the Posts Component

To display the posts on the index page, I created a posts component:

```tsx
import { allPosts, Post as PostType } from 'contentlayer/generated';
import { compareDesc } from 'date-fns';
import { format } from 'date-fns/format';
import { ptBR } from 'date-fns/locale';
import Link from 'next/link';

export default function Posts() {
  const posts = allPosts.sort((a, b) => compareDesc(new Date(a.date), new Date(b.date)))

  return (
    <section className="posts">
      {posts.map((post: PostType, key) => {
        return <Post post={post} key={key}/>
      })}
    </section>
  )
}

function Post({ post }: { post: PostType }) {
  return (
    <article>
      <header>
        <span className="date">{format(post.date, "dd 'de' MMMM 'de' yyyy", { locale: ptBR })}</span>
        <h2>
          <Link href={post.slug}>{post.title}</Link>
        </h2>
      </header>
      <Link href={post.slug} className="image fit">
        <img src={post.mainPicture} alt="" />
      </Link>
      <p>{post.description}</p>
      <ul className="actions special">
        <li key={1}>
          <Link href={post.slug} className="button">Ver mais</Link>
        </li>
      </ul>
    </article>
  )
}
```

The allPosts and PostType imports from the contentlayer package streamline this process.

By far, here is our index page:
![](/images/Snapshot3.png)

## The Posts Route

Creating the posts route involved displaying a post, depending on the route. It's by far the most complex code of the project, and because of the time constraints, I didn't write it as well as I could have. The code turned out functional, but there are several areas that need improvement.
![](https://devs.lol/uploads/2021/11/meme-dev-humor-when-you-write-bad-code-but-it-does-the-job-110.jpg)

```ts
// src/lib/post-image-finder.ts

import path from "path";
import fs from "fs";

export default function PostImagesFinder(slug: string) {
  const postDirectory = path.join(process.cwd(), 'public', slug);
  const imagesDirectories = fs.readdirSync(postDirectory).filter((file) =>
    fs.statSync(path.join(postDirectory, file)).isDirectory()
  )

  const images = imagesDirectories.map((imageDirectory) => {
    const directory = path.join(postDirectory, imageDirectory)
    const files = fs.readdirSync(directory).map((file) => {
      const full = path.join(directory, file)
      const fullSplit = full.split('/')

      return path.join(...fullSplit.slice(Math.max(fullSplit.length - 3, 1)))
    })

    return files
  })

  return images
}

```

This function finds and organizes all the images for the gallery.

```tsx
// src/app/posts/[slug]/page.tsx
import PostImagesFinder from '@/app/lib/post-images-finder';
import { allPosts, Post } from 'contentlayer/generated';
import { format } from 'date-fns/format';
import { ptBR } from 'date-fns/locale';

export async function generateStaticParams() {
  return allPosts.map((post) => ({ slug: post._raw.flattenedPath }))
}

export function generateMetadata({ params }: { params: { slug: string } }) {
  const post = allPosts.find((post) => post._raw.flattenedPath === params.slug)
  if (!post) throw new Error(`Post not found for slug: ${params.slug}`)
  return { title: post.title }
}

export default async function PostLayout({ params }: { params: { slug: string } }) {
  const post = allPosts.find((post) => post._raw.flattenedPath === params.slug)
  if (!post) throw new Error(`Post not found for slug: ${params.slug}`)

  const gallery = PostImagesFinder(post.slug)

  return (
    <section className="post">
      <PostHeader post={post} />
      <PostBody post={post} />
      <PostGallery gallery={gallery} />
    </section>
  )
}

function PostHeader({ post }: { post: Post }) {
  return (
    <header className="major">
      <span className="date">
        {format(post.date, "dd 'de' MMMM 'de' yyyy", { locale: ptBR })}
      </span>
      <h1>{post.title}</h1>
      <p>{post.description}</p>
      <div className="image main">
        <img src={post.mainPicture} alt="" />
      </div>
    </header>
  )
}

function PostBody({ post }: { post: Post }) {
  return (
    <div>
      <span className="image left">
        <img src={post.sidePicture} alt="" />
      </span>
      <span dangerouslySetInnerHTML={{ __html: post.body.html }} />
    </div>
  )
}

function PostGallery({ gallery }: { gallery: string[][] }) {
  return (
    <div>
      {gallery.map((imageCollection, key) =>
        <PostGalleryImageCollection imageCollection={imageCollection} key={key} />
      )}
    </div>
  )
}

function PostGalleryImageCollection({ imageCollection }: { imageCollection: string[] }) {
  return (
    <div className="box alt">
      <div className="row gtr-50 gtr-uniform" style={{justifyContent: "center"}}>
        {imageCollection.map((image, key) =>
          <PostGalleryImage image={image} key={key} />
        )}
      </div>
    </div>
  )
}

function PostGalleryImage({ image }: { image: string }) {
  return (
    <div className="col-4">
      <span className="image fit">
        <img src={image} alt="" />
      </span>
    </div>
  )
}
```

This code handles the display of the post and its associated image gallery.

And here is the post page.

![](/images/Snapshot4.png)

## Conclusion

I successfully migrated the site to NextJs within a day, and it's now live on [bilhalba.com.br](https://bilhalba.com.br), hosted on [Github](https://github.com/TheDevick/BilhalbaEngenharia) and deployed at Vercel. While the project was a success, there are areas for improvement which I plan to address in the coming days. If you're interested, feel free to check it out and follow along with the updates.
