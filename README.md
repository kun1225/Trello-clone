## ✏️ Note

### Clerk - afterAuth
Clerk 可以搭配 Next 的 middleware.ts 根據用戶權限來 redirect，例如：
```js
export default authMiddleware({
  // Allow signed out users to access the specified routes:
  publicRoutes: ['/'],
  afterAuth (auth, req)  {
    if (auth.userId && auth.isPublicRoute) {
      let path = '/select-org';

      if (auth.orgId) {
        path = `/organization/${auth.orgId}`
      }

      const orgSelected = new URL(path, req.url);
      return NextResponse.redirect(orgSelected)
    }

    if (!auth.userId && !auth.isPublicRoute) {
      return redirectToSignIn({ returnBackUrl: req.url })
    }

    if (auth.userId && !auth.orgId && req.nextUrl.pathname !== '/select-org') {
      const orgSelected = new URL('/select-org', req.url);
      return NextResponse.redirect(orgSelected)
    }
  }
});
```

### Next - img remotePatterns
有其它網域來的圖片，記得在 Next 裡設定

```js
// next.config.js
const nextConfig = {
  images: {
    remotePatterns: [
      {
        protocol: "https",
        hostname: "img.clerk.com",
      }
    ]
  }
};

export default nextConfig;
```