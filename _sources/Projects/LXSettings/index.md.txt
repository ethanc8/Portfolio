# LXSettings

<https://github.com/ethanc8/LXSettings>

For much of the COVID-19 pandemic, I was using a Raspberry Pi running Rapsberry Pi Desktop, a fork of LXDE, as my daily driver. RPD was generally a very lightweight desktop that had everything I needed, except that it was quite difficult to configure as it had no settings application like most other desktop environments. In order to solve this I wrote a Gtk application in Python that exposed the different settings applications to the user.

Some settings applications were embeddable using XEmbed, so I set it up so that they could be embedded using XEmbed. Some systems did not even have settings pages, so I exposed their configuration files and dconf key-value stores to the user. Some systems had web interfaces, which I opened in the user's web browser.

Here are some screenshots which I took on KDE Plasma:

![The main page](image.png)
