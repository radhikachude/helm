"# helm"
jobs:

deploy-to-prod:
needs: build
runs-on: ip-172-31-21-21 # Use appropriate OS

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2

      # - name: Login to Docker Hub
      #   run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u ${{ secrets.DOCKER_USERNAME }} --password-stdin

      - name: Pull Image from Docker Hub
        run: docker pull radhikaschude/ScrumMasterUserManagment:latest

      - name: Stop and Remove Old Container
        run: |
          docker stop springboot-example-container || true
          docker rm springboot-example-container || true

      - name: Run Docker Container
        run: |
          docker run -d -p 8080:8080 --restart unless-stopped --name springboot-example-container radhikaschude/ScrumMasterUserManagment:latest
