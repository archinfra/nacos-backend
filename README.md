# nacos-backend

这个仓库用于存放二开后的 Nacos 后端代码，前端代码放在 `archinfra/nacos-frontend`。

## 推送代码

把后端源码放在仓库根目录，根目录需要有 `pom.xml`：

```bash
git clone https://github.com/archinfra/nacos-backend.git
cd nacos-backend
# 把后端源码复制到这里，注意不要复制 console-ui、console-ui-next、console/src/main/resources/static
git add .
git commit -m "feat: import nacos backend"
git push origin main
```

## GitHub Actions 构建

`.github/workflows/backend-ci.yml` 会在 push、PR、手动触发时执行：

```bash
mvn -B -ntp -Prelease-nacos -DskipTests -Dmaven.test.skip=true clean install
```

如果 `-Prelease-nacos` profile 不存在，会自动 fallback 到普通 Maven build。

构建产物会上传到 Actions Artifacts，保留 14 天。主要收集：

- `distribution/target/*.zip`
- `distribution/target/*.tar.gz`
- `console/target/*.jar`
- jar 清单 `ci-artifacts/jars.txt`

## 推荐分仓原则

后端仓库不要提交这些目录：

```text
console-ui/
console-ui-next/
console/src/main/resources/static/
node_modules/
target/
distribution/target/
```

前端独立构建后，后续可以在发布流程里把前端 dist 作为独立 artifact，或者再由部署流水线把前端静态资源拷贝进后端镜像。
