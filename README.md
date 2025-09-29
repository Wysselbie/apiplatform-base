# API Platform Base Docker Image

A comprehensive base Docker image for API Platform applications built on PHP 8.3 FPM with essential system dependencies, PHP extensions, and development tools.

## What's Included

### Base Image
- **PHP 8.3 FPM** - Latest stable PHP with FastCGI Process Manager

### System Dependencies
- **Development tools**: git, curl, zip, unzip
- **Image processing**: libpng-dev, libfreetype6-dev, libjpeg62-turbo-dev
- **Database support**: libpq-dev (PostgreSQL)
- **Text processing**: libonig-dev, libxml2-dev
- **Compression**: libzip-dev
- **Internationalization**: libicu-dev
- **Web server**: nginx
- **Process management**: supervisor

### PHP Extensions
- **Database**: `pdo_mysql`, `pdo_pgsql`
- **Text processing**: `mbstring`, `intl`
- **Image handling**: `gd`, `exif`
- **Math**: `bcmath`
- **System**: `pcntl`, `zip`

### Tools
- **Composer** - Latest version for PHP dependency management

## Usage

### Using in Your Dockerfile

```dockerfile
FROM your-registry/apiplatform-base:latest

# Copy your application configuration
COPY docker/nginx/nginx.conf /etc/nginx/nginx.conf
COPY docker/nginx/default.conf /etc/nginx/conf.d/default.conf
COPY docker/supervisor/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Copy your application code
COPY . /var/www/html

# Set permissions and install dependencies
RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 755 /var/www/html/var

USER www-data
RUN composer install --no-dev --optimize-autoloader

# Configure and start your services
EXPOSE 80
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
```

## Building the Image

### Local Build
```bash
docker build -f Dockerfile.base -t apiplatform-base:latest .
```

### Build with Version Tag
```bash
docker build -f Dockerfile.base -t apiplatform-base:1.0.0 .
```

## Publishing to Registry

### Docker Hub
```bash
# Tag the image
docker tag apiplatform-base:latest your-dockerhub-username/apiplatform-base:php8.3-latest
docker tag apiplatform-base:latest your-dockerhub-username/apiplatform-base:php8.3-1.0.0

# Push to Docker Hub
docker push your-dockerhub-username/apiplatform-base:php8.3-latest
docker push your-dockerhub-username/apiplatform-base:php8.3-1.0.0
```

### GitHub Container Registry
```bash
# Tag the image
docker tag apiplatform-base:latest ghcr.io/your-username/apiplatform-base:php8.3-latest
docker tag apiplatform-base:latest ghcr.io/your-username/apiplatform-base:php8.3-1.0.0

# Push to GitHub Container Registry
docker push ghcr.io/your-username/apiplatform-base:php8.3-latest
docker push ghcr.io/your-username/apiplatform-base:php8.3-1.0.0
```

## Versioning

This image follows semantic versioning:
- **Major versions**: Breaking changes to the base image
- **Minor versions**: New features or significant updates
- **Patch versions**: Bug fixes and security updates

## Supported Tags
- `latest` - Latest stable version
- `php8.3` - Latest PHP 8.3 stable version
- `php8.3-1.0.0` - Specific PHP 8.3 version tags

## Working Directory

The image sets `/var/www/html` as the default working directory, which is the standard location for web applications.

## User Context

The image is designed to run application processes as the `www-data` user for security best practices.

## Requirements

- Docker 20.10+
- Multi-stage build support

## Contributing

1. Fork this repository
2. Create a feature branch
3. Make your changes to `Dockerfile.base`
4. Test the build locally
5. Submit a pull request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

For issues and questions:
- Create an issue in this repository
- Check existing issues for similar problems
- Provide detailed information about your use case

## Related Projects

This base image is designed to work with:
- [API Platform](https://api-platform.com/)
- [Symfony](https://symfony.com/)
- Other PHP-based web applications

---

**Built with ❤️ for the API Platform community**
