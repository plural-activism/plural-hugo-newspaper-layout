# Plural Newspaper - A hugo theme

This is an open-source hugo theme for blogs and informational websites. It is designed to be simple, clean, and easy to use. It is also responsive, meaning it will look great on all devices. The theme is design from the ground up to be accessiable, fast and lightweight. It uses minimal JavaScript and CSS, and it is optimized for performance. It follows the [treblesand colorscheme](https://treblesand.dreamwidth.org/). The mainlayout is a newspaper-style layout, similar to [Hugo Xmag](https://www.gohugothemes.com/theme/yihui-hugo-xmag/), with a sidebar for navigation and a main content area for articles. The theme also includes support for tags and categories, making it easy to organize your content.

## Features

- **General**
  - Built with hugo, a static site generator
  - Easy to customize and extend
  - Supports markdown content
  - Built-in support for tags and categories
  - Responsive design
  - Clean and simple layout
- **Accessibility**
  - The theme is designed to be accessible to all users, including those with disabilities. It follows the [WCAG 2.1 guidelines](https://www.w3.org/TR/WCAG21/) and has been tested with screen readers and other assistive technologies.
  - Three color schemes (light, dark, high contrast)
  - Support for keyboard navigation
  - ARIA roles and attributes for improved accessibility
  - Support for "easy language" content
- **Layout features**
  - Multiple layout options (list, grid, masonry)
  - Supports both static sites and blog posts
  - Dedicated landing page template
  - Customizable header and footer
  - Support for custom widgets and shortcodes
  - Built-in support for social media sharing
- **Developer Features**
  - Uses Github Actions for continuous integration, testing and deployment
  - Uses Hugo's built-in testing framework for unit and integration tests
  - Follows best practices for Hugo theme development
  - Uses Lighthouse for performance and accessibility testing
  - Uses WebPageTest for performance testing
  - Uses Dependabot for dependency management and security updates

## Installation

To install the Plural Newspaper theme, follow these steps:

1. Clone the repository into your hugo site's `themes` directory:

   ```bash
   git clone https://github.com/plural-activism/plural-hugo-newspaper-layout.git themes/plural-hugo-newspaper-layout
    ```

2. Add the following line to your site's `config.toml` file:

    ```toml
    theme = "plural-hugo-newspaper-layout"
    ```

3. Customize the theme by editing the `config.toml` file and adding your content to the `content` directory.
