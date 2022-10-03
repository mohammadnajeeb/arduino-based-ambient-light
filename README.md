<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a name="readme-top"></a>
<!--
*** Thanks for checking out the Best-README-Template. If you have a suggestion
*** that would make this better, please fork the repo and create a pull request
*** or simply open an issue with the tag "enhancement".
*** Don't forget to give the project a star!
*** Thanks again! Now go create something AMAZING! :D
-->



<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->

<!-- PROJECT LOGO -->
<div align="center">

  <h3 align="center">Arduino Based Ambient Light</h3>

  <p align="center">
    Light source that illuminates the wall or surfaces behind the computer screen, reducing eye strain and enhancing image quality.
    <br />
    <a href="https://youtube.com">View Demo</a>
    ·
    <a href="https://github.com/mohammadnajeeb/arduino-based-ambient-light/issues">Report Bug</a>
    ·
    <a href="https://github.com/mohammadnajeeb/arduino-based-ambient-light/issues">Request Feature</a>
  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project

<img src="https://raw.githubusercontent.com/mohammadnajeeb/arduino-based-ambient-light/main/sample/screenshot.jpg" height="300">

An ambient light source is essentially an indirect source of light. It is a soft light that is reflected from the wall or ceiling. The light reduces the shadows on people's faces and fills the room with equal brightness, allowing the object to be more visible and drawing people into the room. Various methods can be used to produce ambient light.

We are using ambient light to reduce eye strain by mounting an Addressable RGB Strip light on the monitor's back. Instead of manually changing the color of the light, here we have used Arduino Nano to control the RGB Strip and display matching colors by grabbing pixels around the monitor. 

<p align="right">(<a href="#readme-top">back to top</a>)</p>



### Built With

* Arduino Nano
* ws2812b LED Strip Light
* C++

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- GETTING STARTED -->
## Getting Started

To get a local copy up and running follow these simple example steps.

### Prerequisites

* Arduino IDE
* FastLED

### Installation

_Below is an example of how you can instruct your audience on installing and setting up your app. This template doesn't rely on any external dependencies or services._

1. Arduino IDE [https://www.arduino.cc/en/software](https://www.arduino.cc/en/software)
2. Paste the code in the IDE
   ```sh
    #include "FastLED.h"
    #define NUM_LEDS 8 
    #define DATA_PIN 7

    #define serialRate 115200

    uint8_t prefix[] = {'A', 'd', 'a'}, hi, lo, chk, i;

    // Initialise LED-array
    CRGB leds[NUM_LEDS];

    void setup() {
      // Use NEOPIXEL to keep true colors
      FastLED.addLeds<WS2812, DATA_PIN>(leds, NUM_LEDS);
      
      // Initial RGB flash
      LEDS.showColor(CRGB(255, 0, 0));
      delay(500);
      LEDS.showColor(CRGB(0, 255, 0));
      delay(500);
      LEDS.showColor(CRGB(0, 0, 255));
      delay(500);
      LEDS.showColor(CRGB(0, 0, 0));
      
      Serial.begin(serialRate);
      // Send "Magic Word" string to host
      Serial.print("Ada\n");
    }

    void loop() { 
      // Wait for first byte of Magic Word
      for(i = 0; i < sizeof prefix; ++i) {
        waitLoop: while (!Serial.available()) ;;
        // Check next byte in Magic Word
        if(prefix[i] == Serial.read()) continue;
        // otherwise, start over
        i = 0;
        goto waitLoop;
      }
      
      // Hi, Lo, Checksum  
      while (!Serial.available()) ;;
      hi=Serial.read();
      while (!Serial.available()) ;;
      lo=Serial.read();
      while (!Serial.available()) ;;
      chk=Serial.read();
      
      // If checksum does not match go back to wait
      if (chk != (hi ^ lo ^ 0x55)) {
        i=0;
        goto waitLoop;
      }
      
      memset(leds, 0, NUM_LEDS * sizeof(struct CRGB));
      // Read the transmission data and set LED values
      for (uint8_t i = 0; i < NUM_LEDS; i++) {
        byte r, g, b;    
        while(!Serial.available());
        r = Serial.read();
        while(!Serial.available());
        g = Serial.read();
        while(!Serial.available());
        b = Serial.read();
        leds[i].r = r;
        leds[i].g = g;
        leds[i].b = b;
      }
      
      // Shows new values
      FastLED.show();
    }
   ```
3. Upload the code to the Arduino

_For full setup instructions, please refer to the [Tutorial Video](https://example.com)_

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- USAGE EXAMPLES -->
## Usage

The ambient light can be used while watching movies or playing games to reduce eye strain and ensure an immersive experience.

_For more examples, please refer to the [Demo](https://example.com)_

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE.txt` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTACT -->
## Contact

Your Name - [@your_twitter](https://twitter.com/your_username) - email@example.com

Project Link: [https://github.com/your_username/repo_name](https://github.com/your_username/repo_name)

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

Use this space to list resources you find helpful and would like to give credit to. I've included a few of my favorites to kick things off!

* [Choose an Open Source License](https://choosealicense.com)
* [GitHub Emoji Cheat Sheet](https://www.webpagefx.com/tools/emoji-cheat-sheet)
* [Malven's Flexbox Cheatsheet](https://flexbox.malven.co/)
* [Malven's Grid Cheatsheet](https://grid.malven.co/)
* [Img Shields](https://shields.io)
* [GitHub Pages](https://pages.github.com)
* [Font Awesome](https://fontawesome.com)
* [React Icons](https://react-icons.github.io/react-icons/search)

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/othneildrew/Best-README-Template.svg?style=for-the-badge
[contributors-url]: https://github.com/othneildrew/Best-README-Template/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/othneildrew/Best-README-Template.svg?style=for-the-badge
[forks-url]: https://github.com/othneildrew/Best-README-Template/network/members
[stars-shield]: https://img.shields.io/github/stars/othneildrew/Best-README-Template.svg?style=for-the-badge
[stars-url]: https://github.com/othneildrew/Best-README-Template/stargazers
[issues-shield]: https://img.shields.io/github/issues/othneildrew/Best-README-Template.svg?style=for-the-badge
[issues-url]: https://github.com/othneildrew/Best-README-Template/issues
[license-shield]: https://img.shields.io/github/license/othneildrew/Best-README-Template.svg?style=for-the-badge
[license-url]: https://github.com/othneildrew/Best-README-Template/blob/master/LICENSE.txt
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/othneildrew
[product-screenshot]: images/screenshot.png
[Next.js]: https://img.shields.io/badge/next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white
[Next-url]: https://nextjs.org/
[React.js]: https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB
[React-url]: https://reactjs.org/
[Vue.js]: https://img.shields.io/badge/Vue.js-35495E?style=for-the-badge&logo=vuedotjs&logoColor=4FC08D
[Vue-url]: https://vuejs.org/
[Angular.io]: https://img.shields.io/badge/Angular-DD0031?style=for-the-badge&logo=angular&logoColor=white
[Angular-url]: https://angular.io/
[Svelte.dev]: https://img.shields.io/badge/Svelte-4A4A55?style=for-the-badge&logo=svelte&logoColor=FF3E00
[Svelte-url]: https://svelte.dev/
[Laravel.com]: https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white
[Laravel-url]: https://laravel.com
[Bootstrap.com]: https://img.shields.io/badge/Bootstrap-563D7C?style=for-the-badge&logo=bootstrap&logoColor=white
[Bootstrap-url]: https://getbootstrap.com
[JQuery.com]: https://img.shields.io/badge/jQuery-0769AD?style=for-the-badge&logo=jquery&logoColor=white
[JQuery-url]: https://jquery.com 
