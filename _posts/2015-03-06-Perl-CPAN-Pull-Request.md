I've been participating in the CPAN pull request challenge in the Perl community this year and it's been a great learning experience. I've learned about Dist::Zilla, Travis CI and Perlbrew. This month, I've been working on html-scrubber, and the Dist::Zilla plugins are giving my windows 8 box a headache so I decided to use Vagrant to spinup a Ubuntu box. Travis uses perlbrew to install a new perl, run the build and tests so why shouldn't I? 

I fired up HyperV, installed a new Ubuntu x64 box, and then fired up Perl. It was set to version 5.14. So here's what I had to do to get up and running:

1. Update Apt-Get
2. Install git (`sudo apt-get install git`) and dependencies
3. Install curl (`sudo apt-get install curl`)
4. Install perlbrew (`sudo apt-get install perlbrew`)
5. Use perlbrew to install Perl 5.20, and "use" it
6. Install CpanMinus
7. Install Dist::Zilla 
  * This was a pain on a new Ubuntu Box. Turns out Path-Tiny just doesn't want to install rightly.