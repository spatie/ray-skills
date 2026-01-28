---
name: spatie-ray-ecosystem
description: Overview of other Spatie Ray repositories and integrations
metadata:
  tags: ray, ecosystem, integrations, packages
---

## Spatie Ray Ecosystem

Ray is available as multiple packages and integrations across different languages and frameworks. This document provides an overview of the Ray ecosystem maintained by Spatie.

### Core Packages

#### spatie/ray (PHP)
**Repository**: https://github.com/spatie/ray  
**Description**: The core PHP package for Ray. Debug with Ray to fix problems faster.  
**Language**: PHP  
**Use when**: Working with any PHP application that doesn't use a specific framework.

#### spatie/laravel-ray (PHP/Laravel)
**Repository**: https://github.com/spatie/laravel-ray  
**Description**: Laravel-specific integration for Ray with additional Laravel helpers.  
**Language**: PHP (Laravel Framework)  
**Use when**: Working with Laravel applications. Includes Laravel-specific debugging features like query logging, model changes, event tracking, and more.

### Framework-Specific Integrations

#### spatie/wordpress-ray (PHP/WordPress)
**Repository**: https://github.com/spatie/wordpress-ray  
**Description**: Debug with Ray to fix problems faster in WordPress apps.  
**Language**: PHP (WordPress)  
**Use when**: Working with WordPress themes or plugins.

#### spatie/craft-ray (PHP/Craft CMS)
**Repository**: https://github.com/spatie/craft-ray  
**Description**: Easily debug CraftCMS projects.  
**Language**: PHP (Craft CMS)  
**Use when**: Working with Craft CMS applications.

#### spatie/yii-ray (PHP/Yii)
**Repository**: https://github.com/spatie/yii-ray  
**Description**: Debug with Ray to fix problems faster in Yii apps.  
**Language**: PHP (Yii Framework)  
**Use when**: Working with Yii2 applications.

#### spatie/drupal-ray (PHP/Drupal)
**Repository**: https://github.com/spatie/drupal-ray  
**Description**: Debug Drupal applications using Ray.  
**Language**: PHP (Drupal)  
**Use when**: Working with Drupal applications.

#### spatie/ray-bundle (PHP/Symfony)
**Repository**: https://github.com/spatie/ray-bundle  
**Description**: A Symfony bundle for Ray.  
**Language**: PHP (Symfony Framework)  
**Use when**: Working with Symfony applications.

### Other Languages

#### spatie/ruby-ray (Ruby)
**Repository**: https://github.com/spatie/ruby-ray  
**Description**: Easily debug Ruby applications.  
**Language**: Ruby  
**Use when**: Working with Ruby or Ruby on Rails applications.

#### spatie/rails-ray (Ruby/Rails)
**Repository**: https://github.com/spatie/rails-ray  
**Description**: Debug Rails apps faster.  
**Language**: Ruby (Rails Framework)  
**Use when**: Working specifically with Ruby on Rails applications.

#### spatie/next-ray (JavaScript/TypeScript)
**Repository**: https://github.com/spatie/next-ray  
**Description**: Ray integration for Next.js applications.  
**Language**: TypeScript/JavaScript (Next.js)  
**Use when**: Working with Next.js applications.

### Utilities

#### spatie/global-ray (PHP)
**Repository**: https://github.com/spatie/global-ray  
**Description**: Enable Ray in all PHP files on your system globally.  
**Language**: PHP  
**Use when**: You want to use Ray functions globally across all PHP projects without installing it per-project.

#### spatie/x-ray (PHP)
**Repository**: https://github.com/spatie/x-ray  
**Description**: Scan source code for calls to ray() and related calls.  
**Language**: PHP  
**Use when**: You want to find and remove Ray debugging calls from your codebase before production deployment.

### Ray Application & Documentation

#### Ray Desktop App
**Website**: https://myray.app/  
**Description**: The Ray desktop application that receives and displays debug information.  
**Platforms**: macOS, Windows, Linux  
**Note**: The desktop app must be running to receive payloads.

#### Ray Documentation
**Website**: https://myray.app/docs/  
**Payload Documentation**: https://myray.app/docs/developing-ray-libraries/payload  
**Description**: Official Ray documentation including guides, API references, and examples.

## Using This Information as an Agent

When a user mentions using Ray with a specific framework or language:

1. **Check if there's a specific integration** - If the user is using Laravel, WordPress, Symfony, etc., mention the framework-specific package as it will have additional helpers.

2. **Reference the appropriate repository** - Point users to the GitHub repository for installation instructions and framework-specific features.

3. **Use the HTTP API as fallback** - If no specific integration exists for a language/framework, you can still use the HTTP API documented in this skill to send payloads to Ray.

4. **Link to official docs** - For detailed information beyond basic usage, refer users to https://myray.app/docs/

## Example Recommendations

**User working with Laravel**:
> "For Laravel applications, I recommend using `spatie/laravel-ray` (https://github.com/spatie/laravel-ray) which includes Laravel-specific helpers like `ray()->showQueries()`, `ray()->showEvents()`, and more. Alternatively, I can send payloads to Ray using the HTTP API documented in this skill."

**User working with Node.js/Next.js**:
> "For Next.js applications, check out `spatie/next-ray` (https://github.com/spatie/next-ray). For other Node.js frameworks, I can send payloads directly to Ray's HTTP API using the methods documented in this skill."

**User working with Python**:
> "There isn't a Python-specific Ray package yet, but I can send debugging information to Ray using its HTTP API. I'll use curl/HTTP requests to send payloads to Ray's local server at http://localhost:23517/"
