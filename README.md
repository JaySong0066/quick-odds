# 快速赔率 · PWA 部署说明

本文件夹就是完整的 App,共 4 类文件:`index.html`(应用本体)、`manifest.webmanifest`(应用清单:名称/图标/全屏模式)、`sw.js`(离线缓存)、`icons/`(应用图标)。**部署到任何支持 HTTPS 的静态托管即可安装使用**,无需服务器代码。

---

## 一、最快路径:GitHub Pages(免费,约 5 分钟)

1. 在 github.com 新建一个仓库(比如 `quick-odds`),把本文件夹里的**所有文件**上传到仓库根目录(网页端直接拖拽即可);
2. 仓库 Settings → Pages → Source 选 `main` 分支根目录,保存;
3. 一两分钟后访问 `https://你的用户名.github.io/quick-odds/` ——完成。

其它同类免费选择:Netlify(把整个文件夹拖进 app.netlify.com/drop 即可,连仓库都不用建)、Vercel、Cloudflare Pages。

## 二、装到手机上

- **iPhone**:Safari 打开网址 → 分享按钮 → 「添加到主屏幕」。图标出现在桌面,点开是全屏独立窗口,之后**断网也能用**。
- **Android**:Chrome 打开网址 → 浏览器会自动弹「安装应用」提示(或菜单 → 安装应用/添加到主屏幕)。

安装后更新方法:改动文件重新部署,并把 `sw.js` 里的 `quick-odds-v1` 改成 `v2`(版本号变化会触发所有已安装设备下次打开时自动更新缓存)。

## 三、要上应用商店?用 Capacitor 套壳(代码零改动)

PWA 不经过 App Store / Google Play。如果之后想要商店曝光与「正式 App」身份:

```bash
npm init -y && npm install @capacitor/core @capacitor/cli
npx cap init "快速赔率" com.yourname.quickodds --web-dir .
npm install @capacitor/ios @capacitor/android
npx cap add ios && npx cap add android
npx cap open ios      # 需要 Mac + Xcode,上架需 Apple 开发者账号 $99/年
npx cap open android  # 需要 Android Studio,Google Play 注册 $25 一次性
```

本文件夹的文件原封不动就是 Capacitor 的 web 资产;震动反馈等原生能力可换成 `@capacitor/haptics` 增强。商店审核提示:定位写「学习/工具类计算器,不涉及真钱游戏」,通过率高。

## 四、常见问题

- **必须 HTTPS 吗?** 是(Service Worker 的硬性要求),上面所有托管都自带 HTTPS。
- **数据存在哪?** 不存任何数据:无历史记录、无账号、无联网请求,关掉即走。
- **体积?** 整个 App 约 40 KB(不含图标),首次打开即完成全部缓存。
