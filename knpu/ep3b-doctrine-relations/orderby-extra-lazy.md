# Orderby Extra Lazy

Coming soon...

So it's really nice that we now have real dynamic comments, comments below each article, but if you look around, you can spot a problem. The articles are not in the right order. We really want the newest article on top and then we went to descend to the oldest oldest art comments on the bottom. The problem is that if you're looking at the template, all we're doing is just calling article that comments, which is article Arrow, get comments in. The great thing about shortcut methods like this is that they're really easy to use. The downside is that you don't have a lot of control over what's returned. Actually that's not true as we'll see, but we'll get to that. In this case, it's just going. It's simply returning the comments in whatever order they were loaded into the database, and while we don't have a lot of control over these fields, we can control the order of them in articles. Scroll all the way to the top and find the comments property below the one it's money at another annotation called at rm slash order by and say curly brace created at equals descending. 

Then go back, refresh, and perfect. Newest comments are on top. I want to say two quick things about the syntax for the annotation here. First of all, the syntax for annotations in general can sometimes be a little confusing. This curly brace here is setting up an a in associative array. Don't worry about it too much even. I still need to sometimes look up the correct syntax for different situations with annotations. The second thing is, for better or worse, annotations only support double quotes, so don't you can't ever use single quotes. It just doesn't work. All right. I want to show you one more trick. Go back to the homepage when we. Let's start articles here. It'd be really cool to list the number of comments on each article, so that's easy to do. Open home page, that html Twig, and then when you loop over the articles right after the article title, let's add a small tag that'll add parentheses and we'll say curly curly article dot comments. We use that same helper method, pipe length through count those comments, and then we'll put the word comments at the end of it. Go back, refresh and perfect it works effortlessly, but check out the comments down here in the web debug toolbar. If you click into it, there are actually six comments now, the first query for six queries, the queries why you'd expect this is the queer that's selecting all of the articles that are published. The other five queries are actually selecting. 

Second mcquery selects all of the comments for an article whose ideas one, 76 and the next queries selects all the comments for another article and then we select all of the comments for another article. What's happening here is as we loop over our table, each time we call get comments at that moment, doctrine goes and fetches all of the comments for that specific article so that we can get a count. There are two problems with this. First, we now have six queries which is inefficient. This is known as the in plus. One. Problem is the problem and it's a problem with thanks to the magic of doctrine. When you're looping over many entities like articles and you're fetching data from a related article, you might end up with a lot of queries, one extra queries. We are going to talk about solving that, but it's not always that big of a deal. Six periods on this page is not necessarily a huge deal, but there's a second way that you can think about this problem. Let's say that we're OK with making [inaudible] queries. One querie per row on this page. The problem is that the query is way too big. 

It's actually selecting all of the comment data just so that it can get a count of the comments. This is the default behavior of doctrine. As soon as you call, as soon as you call get comments and use that data, it makes a query at that moment to get all of the comment data, but we can control that behavior in the article entity at the end of the [inaudible] annotation, add fetch equals extra lazy. Now go back to our page refresh and you'll see that we still have six queries, but if you look at the queries, the five extra queries are super fast select count queries now, so instead of fetching all of the data, it's just fetching the count of the data. So here's how fetch extra lazy works if you have this setting on. 

Yeah. 

Then when you call get comments, if you only count the comments, then instead of querying for all of the it just does a quick count query, which in this case is exactly what we want. In fact, if you don't think about it too hard, it seems like fetch extra lazy is actually the best setting for all of the time and that's almost true. The one situation where having fetch extra lazy is not good is if you intend to, to the comments and the then loop over them and print them. 

So in our case, if you look at a specific, we're here. It's exactly what we do in our article show page. We count our queries and then we loop over them. So if you look at the portfolio now, thanks to the extra lazy, we actually have this extra queer here that counts them and then we loop over the comments before we had extra lazy, we just had this queer here which was used for the count and then use to fetch the data. So using extra lazy is a trade off. It just depends on your on your situation and helps us on the home page, but then hurts us on the article show page if you're going to be doing a lot of counting of a relationship without actually printing that relationships data, it's probably a good idea. Either way. These are minor performance optimizations, so don't prematurely optimize things too much.