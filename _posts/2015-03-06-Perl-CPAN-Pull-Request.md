---
layout: post
title: "Utilizing Vagrant for the CPAN Pull Request Challenge"
---

# Utilizing Vagrant for the CPAN Pull Request Challenge
I've been participating in the [CPAN pull request challenge](http://perlmaven.com/2015-cpan-pull-request-challenge) in the Perl community this year and it's been a great learning experience. I've learned about [`Dist::Zilla`](https://metacpan.org/release/Dist-Zilla), [Travis CI](http://travis-ci.org) and [`Perlbrew`](https://metacpan.org/pod/distribution/App-perlbrew/bin/perlbrew). This month, I've been working on [`html-scrubber`](http://metacpan.org/release/html-scrubber), and the `Dist::Zilla` plugins are giving my windows 8 box a headache so I decided to use Vagrant to spinup a Ubuntu box. Travis uses perlbrew to install a new perl, run the build and tests so why shouldn't I? 

I installed ~~HyperV~~ VirtualBox, initialized a new [ubuntu/trusty64](https://vagrantcloud.com/ubuntu/boxes/trusty64) box (`vagrant init ubuntu/trusty64`, `vagrant up`), and then fired up Perl. It was set to version 5.14. So here's what I had to do to get up and running:

1. Update Apt-Get
2. Install git (`sudo apt-get install git`) and dependencies
3. Install libssl headers & includes (`sudo apt-get install libssl-dev`) (for Dist::Zilla deps)
4. Install perlbrew (`sudo apt-get install perlbrew`)
5. Use perlbrew to install Perl 5.20, and "use" it
  1. `perlbrew init`
  2. `perlbrew install stable`
  3. `perlbrew use perl-5.20.2`
6. Install CpanMinus
  1. `perlbrew install-cpanm`
7. Create a library home for new repos to sandbox things and use it 
  1. `perlbrew lib create mrc`
  2. `perlbrew use perl-5.20.2@mrc`
  3. `export PATH=$PATH:$PERLBREW_PATH` (needed to include the bin for `dzil`)
7. Install Dist::Zilla 
  2. `cpanm -n Dist::Zilla`

## Update
After some frustration with Hyper-V and proxy, I decided to go back to VirtualBox. I was having some issues getting through my CNTLM proxy _still_. I had to setup an additional private NIC and then setup a firewall rule to allow the VM to connect to my CNTLM proxy. Still wasn't working. Then I discovered that I had to turn on the `-g` (gateway) option in CNTLM, allowing it to be accessed from *.*.*.*:3128, not just the loopback device. That worked! Now I'm back up (with VirtualBox), proxy-enabled and sharing folders.
