# n8n Workflow Automation

This project provides a Docker setup for running n8n, a powerful workflow automation tool that allows you to connect various services and automate tasks.

## Description

This repository contains Docker configurations to easily deploy n8n. n8n is an open-source workflow automation tool that enables you to create complex workflows by connecting different services and APIs without writing code.

## Services

- **n8n**: The main service that runs the n8n workflow automation tool.
  - **Container Name**: auto
  - **Image**: docker.n8n.io/n8nio/n8n
  - **Ports**: Exposed on port (5678)
  - **Volumes**: Data is persisted in `n8n_data` volume, mapped to `/home/node/.n8n`
  - **Environment Variables**: Loaded from `.env`
  - **Restart Policy**: Always restart the container
  - **Resource Limits**: Memory limit of `1G` and CPU limit of `1`

## Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/yourusername/your-repo.git
   ```

2. Navigate to the project directory:

   ```bash
   cd your-repo
   ```

3. Start the services using Docker Compose:

   ```bash
   docker-compose up -d
   ```

## Usage

- Access n8n at [http://localhost:5678](http://localhost:5678) to create and manage your workflows.

## Contributing

1. Fork the repository.
2. Create a new branch:

   ```bash
   git checkout -b feature/YourFeature
   ```

3. Make your changes and commit them:

   ```bash
   git commit -m "Add your message"
   ```

4. Push to the branch:

   ```bash
   git push origin feature/YourFeature
   ```

5. Open a pull request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
