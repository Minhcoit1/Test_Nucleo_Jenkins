pipeline {
    agent any

    environment {
        PROJECT_NAME = "Test_Nucleo_Jenkins"   // thay bằng tên project CubeIDE của bạn
        BUILD_DIR = "Debug"               // hoặc "Release" tùy bạn chọn trong CubeIDE
    }

    stages {
        stage('Checkout') {
            steps {
                // Lấy code từ Git
                git branch: 'master', url: 'https://github.com/Minhcoit1/Test_Nucleo_Jenkins.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh """
                        echo "=== Building STM32 firmware ==="
                        cd ${PROJECT_NAME}
                        make -j4
                    """
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: "${PROJECT_NAME}/${BUILD_DIR}/*.elf, ${PROJECT_NAME}/${BUILD_DIR}/*.bin", fingerprint: true
            }
        }

        stage('Optional: Flash Board') {
            when {
                expression { return params.FLASH == true }
            }
            steps {
                script {
                    sh """
                        echo "=== Flashing firmware to board ==="
                        st-flash write ${PROJECT_NAME}/${BUILD_DIR}/${PROJECT_NAME}.bin 0x08000000
                    """
                }
            }
        }
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
    }
}
