[[Gitlab]]、[[主題分類/DevOps/Docker]]

 > 參考 
 > 龍哥的影片：[為你自己學 Gitlab CI/CD](https://youtube.com/playlist?list=PLBd8JGCAcUAEwyH2kT1wW2BUmcSPQzGcu)。
 > 自己的 Gitlab repository：[CI CD Demo](https://gitlab.com/ci-cd-demo3/ci-cd-demo)，、[shopping-cat-v2](https://gitlab.com/linche0859/shopping-cat-v2)。
 
# Gitlab Runner
Gitlab runner 類似於經紀人的角色，它會向 Gitlab 詢問是否有需要執行的任務，如果有，則會接收任務、整理和指派給 executor (實際執行任務者)。

## 三種類型的 Runner
除了 Gitlab 內建的 [Shared runners](https://docs.gitlab.com/ee/ci/runners/runners_scope.html#shared-runners) 外，也可以自訂 Runner 供專案需求使用，詳細可以參考 [GitLab Runner 文件](https://docs.gitlab.com/runner/#who-has-access-to-runners-in-the-gitlab-ui)。

## 安裝自訂的 Runner
> 參考 [Install GitLab Runner](https://docs.gitlab.com/runner/install/)。

1. 按照官方文件安裝 `gitlab-runner` 套件後，可以使用下列指令進行註冊。
	```bash
	gitlab-runner register
	```

2. 在 tags 的步驟可以設定多個標籤，如：
	```bash
	do, remote, shell
	```

3. 最後選擇要執行 executor 的環境，如果是用 [Docker Hub Container Image Library](https://hub.docker.com/) 上的 images 話，可以選擇 `docker` 的選項，如果要用自訂的 shell 指令，選擇 `shell` 選項即可。

4. 在專案 `.gitlab-ci.yml` 的 job 中，使用 `tags` 指派是由哪一個 Runner 負責處理：
	```yml
	helloword:
	  tags:
	    - remote
	```

	如果前面在註冊 Runner 時，executor 的執行環境是選擇 `docker`，則可以撰寫 `images`，定義在執行環境：
	```yml
	run_node_script:
	  # Group runners 或 Specific runners
	  tags:
	    - sincyuan
	  # 指定 docker executor image 版本
	  image: node:18-alpine3.15
	```

# 變數
> 內建的 Gitlab 變數：[Predefined variables reference](https://docs.gitlab.com/ee/ci/variables/predefined_variables.html)。

## 自訂變數
- `.gitlab-ci.yml` 的全域變數、區域變數：
	```yml
	# 全域的變數
	variables:
	  TEST_VAR: "All jobs can use this variable's value"
	# 區域變數 (只能在這個 job 內使用)
	job1: 
		variables: 
			TEST_VAR_JOB: "Only job1 can use this variable's value"
	```

- Gitlab 平台的全域變數 - 在 Settings -> CI/CD -> Variables 中設定。

# Container Registry
用於存放打包好的 docker image，常會用不同的版本標籤，表示打包後的版本號，可以參考練習的 [Container Registry](https://gitlab.com/linche0859/shopping-cat-v2/container_registry/3485337)。









 
 
 