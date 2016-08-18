A couple of weeks ago I discovered that in addition to regular things you can
install complete apps that have anything even vaguely to do with programming
using:

    brew cask install

And I mean vaguely. The complete list of the things I have brew installed with:

    brew cask list

is:

    alfred			       firefox			      java			     sourcetree			    ubersicht
    atom			       google-chrome		      keepassx			     spotify			    virtualbox
    atom-beta		       intellij-idea		      malwarebytes-anti-malware	spotify-notifications	    visualvm
    betterzip		       intellij-idea-ce		      pgadmin3			     sublime-text
    cmake			       iterm2			      postico			     sublime-text3
    filezilla		       iterm2-beta		      slack			     tunnelblick

Note all of the random crap like spotify.
And of course using brew, I've gotten used to automatically updating
everything with :

    brew upgrade -all

However, as documenented in a couple of places on the internet, this functionality does not exist for brew cask. Instead, you have to use:

    brew cask install --force $nameofprogram

As a programmer, of course doing all of this manually seems like too much trouble. I spent some time on Friday writing a simple shell script that would execute the brew force install on each app that had been cask installed. This is not an ideal solution for a number of reasons, but it is functional.
That script looks like so:

    #!/bin/bash
    for program in `brew cask list`

    do
      echo $program
      brew cask install --force "$program"
    done

One of the other problems with installing apps in this way is that the update creates a second app in your homebrew folder, which means that you get obnoxious dupes in your spotlight search. I started working on a script that would remove the older folder in the apporpriate directory if there were more than one. This is as far as I got in shell script:

    #!/bin/bash
    for program in `brew cask list`

    do
      echo $program
      brew cask install --force "$program"
      for version in `ls -trh /opt/homebrew-cask/Caskroom/$program`

      do
        echo $version
      done
    done

Obviously this doesn't really do anything beyond printing the version of the Cask install. This was a far as I got because shell script is awful. Instead, I worked out how to make groovy, which I already write, interact with the command line, and wrote something that does what I wanted in Groovy.

    def programList = "brew cask list".execute().text.split("\n")

    programList.each { program ->
      println "$program"
      println "brew cask install --force $program".execute().text
      def versionList = "ls -trh /opt/homebrew-cask/Caskroom/$program".execute().text.split("\n")
      if (versionList.size() > 1) {
        def removeFolder = { i ->
          println "Removing version: ${versionList[i]}"
          println "rm -rf /opt/homebrew-cask/Caskroom/$program/${versionList[i]}".execute().text
        }
        0.upto(versionList.size() - 2, removeFolder)
      }
    }

  This works reasonably well for what I want to do, especially when the script is linked to a short alias that runs the script. Still needs work, but I am happy about it for now.

  *Cheers,*  
  Michael
