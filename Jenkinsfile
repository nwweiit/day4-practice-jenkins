// ✅ Jenkins Declarative Pipeline (Node.js 프로젝트용)

pipeline {
    // 🧩 Jenkins가 어떤 에이전트(노드)에서 이 파이프라인을 실행할지 지정
    // "any"는 빌드 가능한 아무 노드에서나 실행 가능함
    agent any

    // 🌍 파이프라인 전체에서 공통으로 사용할 환경 변수 설정
    // environment {
    //     // macOS/Homebrew 환경에서 Node.js, npm 등이 설치된 경로를 인식하도록 PATH 재설정
    //     //PATH = "/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    // }

    // 🏗️ 실제 작업 단계를 정의하는 블록
    stages {

        // 🗂️ 1️⃣ Git 저장소에서 소스 코드 체크아웃
        stage('Checkout') {
            steps {
                // Jenkins가 설정된 SCM(Source Control Management)에서 자동으로 코드 가져오기
                checkout scm
            }
        }

        // 📦 2️⃣ Node.js 의존성 설치
        stage('Install') {
            steps {
                // package.json에 정의된 모든 npm 패키지 설치
                bat'npm install'
            }
        }

        // 🧪 3️⃣ 테스트 실행
        stage('Test') {
            steps {
                // npm test 명령어 실행 (package.json의 "test" 스크립트 사용)
                bat'npm test' --passWithNoTests
            }
        }

        // 🚀 4️⃣ 애플리케이션 실행 (main 브랜치일 때만)
        stage('Start') {
            
            // ✅ 실행 조건 설정
            when {
                anyOf {                    
                    // 현재 브랜치가 "main"이거나
                    branch 'main'
                    
                    // Git 브랜치 이름이 "origin/main"일 때만 stage 실행
                    // Jenkins internally calls scm.branches and populates env.BRANCH_NAME automatically.
                    expression { env.GIT_BRANCH == 'origin/main' }
                }
            }

            steps {
                // npm start 명령 실행 (보통 서버 시작 또는 빌드 스크립트)
                bat'npm start'
            }
        }
    }

    // 📋 파이프라인 완료 후 실행할 후처리(post) 블록
    post {
        // ✅ 모든 단계가 성공적으로 완료되었을 때 실행
        success {
            echo 'Pipeline 성공적으로 완료!'
        }

        // ❌ 하나라도 실패했을 때 실행
        failure {
            echo 'Pipeline 실패!'
        }
    }
}
