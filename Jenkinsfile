node {
    checkout scm
     docker.withRegistry('http://localhost:5000')
     {

        def customImage = docker.build("my-image:${env.BUILD_ID}")
        customImage.push()

        customImage.push('latest')

     }

}
