node {
    // ✅ 환경 변수 설정
    //env.PATH = "/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"

    try {
        stage('Checkout') {
            // Git 저장소 체크아웃
            checkout scm
        }

        stage('Install') {
            // 의존성 설치
            bat 'npm install'
        }

        stage('Test') {
            // 테스트 실행
            bat 'npm test'
        }

        stage('Start') {
            // 현재 브랜치 정보 수동 확인
            def branch = env.GIT_BRANCH ?: 'unknown'
            echo "현재 브랜치: ${branch}"
    
            // 브랜치 정보가 없거나 unknown이면 main으로 간주
            if (branch == 'main' || branch == 'origin/main' || branch == 'unknown') {
                echo "브랜치 정보가 없거나 main으로 간주됨 → npm start 실행"
                bat 'npm start'
            } else {
                echo "Start stage skipped (현재 브랜치가 main이 아님)"
            }
        }

        // ✅ 성공 시 메시지
        echo 'Pipeline 성공적으로 완료!'

    } catch (err) {
        // ❌ 실패 시 메시지
        echo 'Pipeline 실패!'
        echo "에러 내용: ${err}"
        currentBuild.result = 'FAILURE'
        throw err
    }
    
}
