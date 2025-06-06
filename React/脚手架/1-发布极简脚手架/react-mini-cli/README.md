# 发布极简脚手架

## 项目结构

```plaintext
react-mini-cli/
├── bin/                          # 脚手架执行目录
│   └── react-mini.js             # 核心执行脚本（处理命令行逻辑）
├── template/                     # 项目模板目录（基于Vite+React）
│   ├── public/                   # 公共静态资源（直接复制至生成项目）
│   ├── src/                      # 源码目录
│   │   ├── App.jsx               # React根组件（业务逻辑入口）
│   │   └── index.jsx             # 应用入口（渲染组件到DOM）
│   ├── index.html                # HTML模板（含Vite资源注入标签）
│   ├── .gitignore                # Git忽略配置（排除node_modules等）
│   ├── package.json              # 生成项目的依赖清单（含React/Vite）
│   ├── vite.config.js            # Vite配置文件（可自定义打包规则）
│   └── README.md                 # 生成项目的说明文档
├── package.json                  # 脚手架自身依赖（如commander、fs-extra等）
└── README.md                     # 脚手架使用文档（安装/命令说明）
```

## 1.创建脚手架项目

```bash
mkdir react-mini-cli && cd react-mini-cli
npm init -y # 生成默认的package.json
```

## 2.创建基础文件

```bash
mkdir -p bin template/{public,src}
touch bin/react-mini.js template/{index.html,.gitignore,package.json,vite.config.js,README.md} template/src/{index.jsx,App.jsx}
```

## 3.编写执行脚本

在 bin/react-mini.js 中添加：

```javascript
#!/usr/bin/env node
const { program } = require("commander");
const { version } = require("../package.json");
const path = require("path");
const fs = require("fs-extra");

program
	.version(version)
	.command("create <project-name>")
	.description("创建一个仅包含React的极简项目")
	.action((projectName) => {
		const targetDir = path.join(process.cwd(), projectName);
		const templateDir = path.join(__dirname, "../template");
		try {
			// 复制模板文件
			fs.copySync(templateDir, targetDir);

			// 更新 package.json 中的项目名称
			const packageJsonPath = path.join(targetDir, "package.json");
			if (fs.existsSync(packageJsonPath)) {
				const packageJson = fs.readJsonSync(packageJsonPath);
				packageJson.name = projectName;
				fs.writeJsonSync(packageJsonPath, packageJson, { spaces: 2 });
			}

			console.log(`✨ 成功创建项目 ${projectName}`);
			console.log("👉 要进入项目目录并安装依赖，复制并执行以下命令:");
			console.log(`\n\x1b[36mcd ${projectName} && pnpm install\x1b[0m\n`);
			console.log("👉 项目创建完成后，你可以:");
			console.log("  1. 启动开发服务器: pnpm dev");
			console.log("  2. 构建生产版本: pnpm build");
			console.log("  3. 预览生产版本: pnpm preview");
		} catch (error) {
			console.error(`❌ 创建项目失败: ${error.message}`);
			process.exit(1);
		}
	});

program.parse(process.argv);
```

## 4.编写模版文件

template/src/App.jsx

```javascript
function App() {
	return (
		<div>
			↖（￣^￣）↗ ↖（￣^￣）↗ ↖（￣^￣）↗
			<span>致敬伟大的CodeGod-吕威鹏</span>
			↖（￣^￣）↗ ↖（￣^￣）↗ ↖（￣^￣）↗
		</div>
	);
}
export default App;
```

template/src/index.jsx

```javascript
import ReactDOM from "react-dom/client";
import App from "./App";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);
```

template/index.html

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<title>React-Mini-App</title>
	</head>
	<body>
		<div id="root"></div>
		<script type="module" src="/src/index"></script>
	</body>
</html>
```

template/vite.config.js

```javascript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

// https://vite.dev/config/
export default defineConfig({
	plugins: [react()],
	base: "./",
	server: {
		open: true, // 启动后自动打开浏览器
	},
	resolve: {
		extensions: [".js", ".jsx", ".ts", ".tsx", ".json"], // 按顺序尝试解析的扩展名
	},
});
```

template/package.json

```json
{
	"name": "react-mini-template",
	"private": true,
	"version": "0.0.0",
	"type": "module",
	"scripts": {
		"dev": "vite",
		"build": "vite build",
		"preview": "vite preview"
	},
	"dependencies": {
		"react": "^19.1.0",
		"react-dom": "^19.1.0"
	},
	"devDependencies": {
		"@vitejs/plugin-react": "^4.4.1",
		"vite": "^6.3.5"
	}
}
```

最后生成 template/public/favico.ico（启动项目时 vite 会默认引用）和 template/README.md

## 5.配置脚手架的 package.json

```json
{
	"name": "react-mini-cli",
	"version": "1.0.0",
	"author": "CodeGod-吕威鹏",
	"license": "ISC",
	"description": "仅包含React的极简脚手架，适用于学习和自由扩展",
	"keywords": ["vite", "react", "mini", "minimalist", "极简"],
	"bin": {
		"react-mini": "bin/react-mini.js"
	},
	"files": ["bin", "template"],
	"dependencies": {
		"commander": "^11.1.0",
		"fs-extra": "^11.1.1"
	}
}
```

## 6.本地测试

```bash
# 链接到全局
pnpm link

# 创建测试项目
react-mini create my-app
cd my-app && pnpm install && pnpm run dev
```

## 7.发布到 npm

```bash
npm login  # 登录 NPM 账号
npm publish
```

## 8.使用脚手架

```bash
pnpm install -g react-mini-cli # 发布的脚手架名称
react-mini create my-app
cd my-app && pnpm install && pnpm dev
```
