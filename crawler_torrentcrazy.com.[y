from urllib import urlopen, urlretrieve
from lxml.html import parse
from lxml.cssselect import CSSSelector

"""User defined inputs start here"""

DOWNLOAD_DIR = "."
QUERY = "games"
MAX_TORRENTS = 10

"""User defined inputs end here"""

#-----------------------------------------------------
def get_torrents_from_url(url, server_root):

    """Pass a url such as 'torrentcrazy.com/s/songs' and it will download all torrents from here"""

    global current_page_num
    global global_counter

    print "page : %d" % current_page_num
    print url
    print "---------------------"
    
    search_page = parse(urlopen(url)).getroot()
    link_sel = CSSSelector(".torrents td.l a:nth-child(1)")
    links = link_sel(search_page)

    print "Items found %d\n" % len(links)
    
    for item in links:
        link = item.get("href")

        if not link.endswith(".torrent"):   #there are some other links picked up as well...
            continue
        
        print "%d => Downloading file %s\n" % (global_counter, link)
        
        #local_name = link.split("/")[-1]
        local_name = link[link.rindex("/")+1 :]         #find last "/"
        urlretrieve(link, DOWNLOAD_DIR + "/" + local_name)

        global_counter += 1

        if global_counter > MAX_TORRENTS:               #limit reached...
            return
            
    pages_urls = pages_sel(search_page)                 

    if current_page_num < len(pages_urls) :         #check if we have more pages to crawl...
        current_page_num += 1

        new_page_url = server_root + "s/" + QUERY + "/" + str(current_page_num)
        get_torrents_from_url(new_page_url, server_root)
   

#-----------------------------------------------------
server_root = "http://torrentcrazy.com/"
current_page_num = 1
global_counter = 1

pages_sel = CSSSelector("#pages a")
url = server_root + "s/" + QUERY

print "Searching..."
get_torrents_from_url(url, server_root)

