# next-js-interview-preparation

# 🟢 Basic Level Questions
What is Next.js and why would you use it over React?

What are the main features of Next.js?

What is the difference between SSR and CSR in the context of Next.js?

Explain the purpose of the pages directory in Next.js.

What is file-based routing in Next.js?

How do you create a dynamic route in Next.js?

What is the difference between getStaticProps and getServerSideProps?

What is getStaticPaths and when is it used?

How can you navigate between pages in Next.js?

What is the use of the Link component in Next.js?

# 🟡 Intermediate Level Questions

How does Image Optimization work in Next.js?

What is the difference between API routes and traditional REST APIs?

Explain how Next.js handles static file serving.

What is Incremental Static Regeneration (ISR)? How does it work?

How do you handle custom 404 and 500 pages in Next.js?

What is middleware in Next.js, and what are its common use cases?

Can you explain how next.config.js works and some common configurations?

How do you implement authentication in a Next.js app?

What is the role of _app.js and _document.js files?

How would you fetch data client-side vs server-side in Next.js?

# 🔴 Advanced Level Questions

Explain how React Server Components are used in Next.js (especially in App Router).

What are the benefits of the App Router over the Pages Router in Next.js 13+?

How would you implement internationalization (i18n) in Next.js?

How does streaming and partial rendering work in Next.js 13+?

Describe the performance optimization techniques in Next.js.

How does Next.js handle code-splitting and lazy loading?

How do you deploy a Next.js application (e.g., to Vercel, AWS, Docker)?

How can you integrate GraphQL or REST APIs with Next.js?

Explain Middleware vs API Routes in Next.js. When would you use each?

Can you explain the full rendering lifecycle for a page in Next.js with getStaticProps, getStaticPaths, and ISR?


# 1. What is Next.js and why would you use it over React?
 Answer:
 Next.js is a React-based framework that enables features like server-side rendering (SSR), static site generation (SSG), file-based routing, and API routes out of the box.
 While React focuses on the view layer of the application, Next.js provides a complete framework for building production-ready applications with better SEO, performance, and scalability.

# 2. What are the main features of Next.js?
 Answer:
 Some core features include:
 Server-Side Rendering (SSR)
 Static Site Generation (SSG)
 Incremental Static Regeneration (ISR)
 API routes
 Image optimization
 File-based routing
 Built-in CSS and Sass support
 Middleware and edge functions
 TypeScript support

# 3. What is the difference between SSR and CSR in Next.js?
 Answer:
 SSR (Server-Side Rendering): The page is rendered on the server for each request (getServerSideProps). Good for dynamic content and SEO.
 CSR (Client-Side Rendering): The page is rendered in the browser after JavaScript loads. It's faster after the initial load but not SEO-friendly by default.

# 4. Explain the purpose of the pages directory in Next.js.
 Answer:
 The pages directory defines the application's routes. Each file inside pages automatically becomes a route. For example, pages/about.js becomes accessible at /about. This is known as file-based 
 routing.

## 5. What is file-based routing in Next.js?
 Answer:
 File-based routing means that the file structure inside the pages folder determines the routes of the application. For instance, pages/blog/index.js would map to /blog.

## 6. How do you create a dynamic route in Next.js?
 Answer:
 You create a dynamic route by using square brackets in the file name.
 Example: pages/post/[id].js handles routes like /post/1, /post/hello, etc.
 You can then access the id using useRouter or getStaticProps/getServerSideProps.

# 7. What is the difference between getStaticProps and getServerSideProps?
 Answer:
 getStaticProps: Runs at build time and generates a static HTML page. Great for pages that don’t change often.
 getServerSideProps: Runs on every request, and generates the page on the server each time. Ideal for dynamic data.



# 8. What is getStaticPaths and when is it used?
 Answer:
 getStaticPaths is used with getStaticProps for dynamic static pages. It defines which paths should be pre-rendered at build time.
 Example: if you have a blog, getStaticPaths defines which blog post pages to generate.

# 9. How can you navigate between pages in Next.js?
 Answer:
 You can navigate using the Link component from next/link or using useRouter().push() for programmatic navigation.
###
  import Link from 'next/link';  
  <Link href="/about">About</Link>
###  

# 10. What is the use of the Link component in Next.js?
 Answer:
 The Link component enables client-side navigation between pages, which is faster than traditional page reloads. It also prefetches the linked page for better performance.

# 11. Real Life Eample of using getStaticPaths and getStaticProps with Markdown files as blog posts.

🗂 Project Structure

###
   /pages
     /posts
       [slug].js       ← dynamic blog route
   /posts
     hello-world.md
     nextjs-tips.md
   /lib
     posts.js          ← helper to read markdown
###

## 2️⃣ /lib/posts.js – Markdown Parser

###
    import fs from 'fs';
    import path from 'path';
    import matter from 'gray-matter';
    
    const postsDirectory = path.join(process.cwd(), 'posts');
    
    export function getAllPostSlugs() {
      const filenames = fs.readdirSync(postsDirectory);
      return filenames.map((filename) => ({
        params: {
          slug: filename.replace(/\.md$/, ''),
        },
      }));
    }
  
    export function getPostData(slug) {
      const fullPath = path.join(postsDirectory, `${slug}.md`);
      const fileContents = fs.readFileSync(fullPath, 'utf8');
      const { data, content } = matter(fileContents);
    
      return {
        slug,
        ...data,
        content,
      };
    }

### 

## 3️⃣ /pages/posts/[slug].js
###

    import { getAllPostSlugs, getPostData } from '../../lib/posts';
    
    export async function getStaticPaths() {
      const paths = getAllPostSlugs();
      return {
        paths,
        fallback: false,
      };
    }
    
    export async function getStaticProps({ params }) {
      const post = getPostData(params.slug);
      return {
        props: {
          post,
        },
      };
    }
    
    export default function Post({ post }) {
      return (
        <article>
          <h1>{post.title}</h1>
          <p>{post.date}</p>
          <div>{post.content}</div>
        </article>
      );
    }
### 

## 🧠 Summary of What Happens
getAllPostSlugs() reads all .md files and returns slugs like:

[{ params: { slug: 'hello-world' } }, { params: { slug: 'nextjs-tips' } }]

Next.js uses these slugs in getStaticPaths to pre-render pages like /posts/hello-world.
For each slug, getStaticProps() reads and parses the .md file and passes the data to the Post component.



## 📦 Output After Build (next build)
Static HTML is created for each post.
Super-fast page loads.
SEO optimized.
No server code needed at runtime.

## 🔄 Visual Flow:

[next build]
    ↓
Detects [id].js → Sees getStaticPaths()
    ↓
Calls getStaticPaths() → Gets paths like /posts/1, /posts/2
    ↓
For each path → Calls getStaticProps({ params: { id } })
    ↓
Gets props → Renders page as HTML + JSON


