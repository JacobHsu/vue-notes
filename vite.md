# vite

```
**PWA**

PWA 的全稱為 "Progressive Web App"，即 漸進式 Web 應用，是一種使用現代 Web 技術建構的應用程式，旨在提供與原生應用類似的使用者體驗。

通過使用 PWA，開發者可以為使用者提供更快速、可靠和具有原生應用等級功能的 Web 應用體驗，同時減少了開發和維護多個平台的複雜性，使應用程式更易於分發和更新。
```

簡單來說，就是能夠讓我們的 Web 應用能夠適配 PC 端和移動端。

那麼在 vite.config.js 檔案中，我們可以先將所有的外掛 "分離" 出來：

```js
// env: ImportMetaEnv
const setupPlugins = (env) => {
  return [
    // 使用vue插件
    vue(),
    // 使用EsLint插件
    EsLintPlugin({
      // 配置EsLint插件包含的文件
      include: ["src/**/*.js", "src/**/*.vue", "src/*.js", "src/*.vue"],
    }),
    // 使用AutoImport插件
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    // 使用Components插件
    Components({
      resolvers: [ElementPlusResolver()],
    }),
    // 当环境变量VITE_GLOB_APP_PWA的值为true时执行以下操作
    env.VITE_GLOB_APP_PWA === "true" &&
      // 使用VitePWA插件
      VitePWA({
        // 自动注入Service Worker注册代码
        injectRegister: "auto",
        // Web App Manifest的配置
        manifest: {
          // 应用名称
          name: "ChatBot",
          // 应用的短名称
          short_name: "ChatBot",
          // 应用图标的配置 包括图标的路径、尺寸和类型
          icons: [
            { src: "pwa-192x192.png", sizes: "192x192", type: "image/png" },
            { src: "pwa-512x512.png", sizes: "512x512", type: "image/png" },
          ],
        },
      }),
  ];
};
```

然後將 `setupPlugins()` 所返回的外掛陣列返回給 plugins 選項即可。

```
export default defineConfig((config) => {
  const env = loadEnv(config.mode, process.cwd());

  return {
    plugins: setupPlugins(env),
    ...
  };
});
```
