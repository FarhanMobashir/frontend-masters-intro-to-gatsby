---
path: "/create-listing-of-third-party-data"
title: "Create Listing of Third-Party Data"
order: "7C"
section: "Use Third-Party Data"
description: "TKTK"
---

`src/pages/index.js`:

```diff
  import * as React from 'react';
  import { Link, useStaticQuery, graphql } from 'gatsby';
  import { StaticImage } from 'gatsby-plugin-image';
  import Layout from '../components/layout';
  import { imageWrapper } from '../styles/index.module.css';

  export default function IndexPage(props) {
    const data = useStaticQuery(graphql`
      query GetBlogPosts {
        allMdx(sort: { order: DESC, fields: frontmatter___date }, limit: 10) {
          nodes {
            id
            slug
            frontmatter {
              title
              description
              date(fromNow: true)
            }
          }
        }
+       allSanityEpisode(
+         sort: { fields: date, order: DESC }
+         filter: { youtubeID: { ne: null } }
+       ) {
+         nodes {
+           title
+           guest {
+             name
+           }
+           gatsbyPath(filePath: "/episode/{SanityEpisode.slug__current}")
+         }
+       }
      }
    `);

    const posts = data.allMdx.nodes;
+   const episodes = data.allSanityEpisode.nodes;

    return (
      <Layout>
        <div className={imageWrapper}>
          <StaticImage
            src="../images/ivana-la-61jg6zviI7I-unsplash.jpg"
            alt="a corgi sitting on a bed with red paper hearts all over it. it looks unamused."
            placeholder="dominantColor"
            width={300}
            height={300}
          />
        </div>
        <h1>Hello Frontend Masters</h1>
        <Link to="/about">About this site</Link>
        <p>Check out my most recent blog posts.</p>
        <ul>
          {posts.map((post) => (
            <li key={post.id}>
              <Link to={post.slug}>{post.frontmatter.title}</Link>{' '}
              <small>posted {post.frontmatter.date}</small>
            </li>
          ))}
        </ul>
+       <h2>
+         Episodes of <em>Learn With Jason</em>
+       </h2>
+       <ul>
+         {episodes.map((episode) => (
+           <li key={episode.gatsbyPath}>
+             <Link to={episode.gatsbyPath}>
+               {episode.title} (with {episode.guest?.[0]?.name})
+             </Link>
+           </li>
+         ))}
+       </ul>
      </Layout>
    );
  }
```
