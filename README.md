# Dylan Lott 

Hi there. This is the git repo for my personal blog. I used Jekyll and Hyde to create this. It was a pretty quick process. 

I'm the owner of [Hivemind Apps](www.hivemindapps.com) and I work with some really talented people at [Zaniac](www.zaniaclearning.com) 

If you have any questions, feel free to hit me up at `lott.dylan@gmail.com` or on [Instagram](www.instagram.com/dylanxedge)

# Super Fast Publishing 

I wanted to setup a very quick way to publish my blog posts and ideas. I did a bit of googling and was able to hack together a script that would auto populate the date in my commits and then I wrote a simple git add and git commit script with those dates. 

So now, in order to publish a blog post all I have to do is enter `blog` which will `cd` me into my blog directory and open up sublime with my blog's files. Then I can start typing. Once I'm done, all I have to do is enter `publish` and it'll auto add, commit, and push all my files up to my github pages repo. 

`~/.bash_profile` 

    alias blog='cd ~/Development/<your_blog_folder> && sublime .'
    alias publish='git add . && git commit -am "publish updates for `date +"%Y-%m-%d"`" && git push origin master'


Hopefully someone finds this useful! 


