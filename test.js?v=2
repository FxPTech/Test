//https://github.com/SilverTuxedo/FxPlusplus
console.info("showForumStats is enabled");

$(".threads_list_fxp").after(
    $("<div>", { id: "forumStatsContainer" }).append(
        $("<div>", { id: "forumStats" }).append(
            $("<i>").text("סטטיסטיקות:")
        )
    )
);
var regex = {
    fullWord: /([^\s.,\/#?!$%\^&\*+;:{}|=\-_`~()]+)/g,
    notNumber: /^\D+/g
};

let params = new URLSearchParams(location.search);
let id = params.get("f") ? params.get('f') : 21;
console.log(id);
httpGetAsync("https://jsonp.afeld.me/?url=https://www.fxp.co.il/forumdisplay.php?f="+id+"&pp=200",httpGetFunction)
//PUBLISHERS
function httpGetFunction(response) { 
    var domParser = new DOMParser();
    var doc = $(domParser.parseFromString(response, "text/html"))
var publishers = []; //array of all posters that posted in the forum
doc.find("#threads .threadinfo .username").each(function ()
{
    publishers.push($(this).text());
});

var publishersDict = getDupeSortedDictionary(publishers);

line = $("<div>");
if (publishersDict.length > 1 && publishersDict[0].count > 1)
{
    line.append($("<span>").text("המפרסם הדומיננטי ביותר הוא "));
    //add names until the count is not the largest
    for (var i = 0; i < publishersDict.length && (publishersDict[i].count === publishersDict[0].count); i++)
    {
        if (i > 0)
            line.append($("<span>").text(" או "));
        line.append($("<b>").text(publishersDict[i].value));
    }
    line.append($("<span>").text(" עם " + publishersDict[0].count + " אשכולות."));
}
else
{
    line.append($("<span>").text("אין מפרסם דומיננטי במיוחד."));
}
$("#forumStats").append(line);

//COMMENTORS

var commentors = []; //array of all commentors that last posted in threads
doc.find("#threads .threadlastpost .username").each(function ()
{
    commentors.push($(this).text());
});

var commentorsDict = getDupeSortedDictionary(commentors);

line = $("<div>");
if (commentorsDict.length > 1 && commentorsDict[0].count > 1)
{
    line.append($("<span>").text("המגיב האחרון הדומיננטי ביותר הוא "));
    for (var i = 0; i < commentorsDict.length && (commentorsDict[i].count === commentorsDict[0].count); i++)
    {
        if (i > 0)
            line.append($("<span>").text(" או "));
        line.append($("<b>").text(commentorsDict[i].value));
    }
    line.append($("<span>").text(" עם " + commentorsDict[0].count + " תגובות אחרונות."));
}
else
{
    line.append($("<span>").text("אין מגיב אחרון דומיננטי במיוחד."));
}
$("#forumStats").append(line);

//WORDS

var words = []; //array of all words in titles
doc.find("#threads .title").each(function ()
{
    var titleWords = $(this).text().match(regex.fullWord); //get words in title
    if (titleWords !== null)
    {
        titleWords.forEach(function (word)
        {
            if (word.length > 1) //push words to the array of all the words
                words.push(word);
        });
    }
});

var wordsDict = getDupeSortedDictionary(words);

var line = $("<div>");
if (wordsDict.length > 1 && wordsDict[0].count > 1)
{
    line.append($("<span>").text("המילה הנפוצה ביותר בכותרות היא "));
    for (var i = 0; i < wordsDict.length && (wordsDict[i].count === wordsDict[0].count); i++)
    {
        if (i > 0)
            line.append($("<span>").text(" או "));
        line.append($("<b>").text(wordsDict[i].value));
    }
    line.append($("<span>").text(" עם " + wordsDict[0].count + " אזכורים."));
}
else
{
    line.append($("<span>").text("אין מילה נפוצה במיוחד בכותרות."));
}
$("#forumStats").append(line);

//PREFIXES

var prefixes = []; //array of all prefixes of threads
doc.find("#threads .prefix").each(function ()
{
    var prefix = $(this).text().trim();
    prefix = prefix.replace("|", "").replace("סקר: ", "").trim();
    prefixes.push(prefix);
});

var prefixesDict = getDupeSortedDictionary(prefixes);

line = $("<div>");
if (prefixesDict.length > 1 && prefixesDict[0].count > 1) 
{
    line.append($("<span>").text("התיוג הנפוץ ביותר הוא "));
    for (var i = 0; i < prefixesDict.length && (prefixesDict[i].count === prefixesDict[0].count); i++)
    {
        if (i > 0)
            line.append($("<span>").text(" או "));
        line.append($("<b>").text(prefixesDict[i].value));
    }
    line.append($("<span>").text(" שנמצא ב-" + prefixesDict[0].count + " אשכולות."));
}
else
{
    line.append($("<span>").text("אין תיוג נפוץ במיוחד."));
}
$("#forumStats").append(line);

//VIEW COMMENT RATIO

var commentsCount = 0;
var viewsCount = 0;

var cc, vc;

doc.find("#threads .threadstats").each(function ()
{
    cc = parseInt($(this).find("li:eq(0)").text().replace(",", "").replace(regex.notNumber, ""));
    vc = parseInt($(this).find("li:eq(1)").text().replace(",", "").replace(regex.notNumber, ""));
    if (!isNaN(cc))
        commentsCount += cc;
    if (!isNaN(vc))
        viewsCount += vc;
});

var viewsCommentsRatio = Math.round(viewsCount / commentsCount);
if (viewsCommentsRatio < 1)
    viewsCommentsRatio = 1; //make sure that it's not rounded to 0

if (isNaN(viewsCommentsRatio))
    viewsCommentsRatio = "∞";

line = $("<div>");
line.append($("<span>").text("יחס הצפיות לתגובה הוא תגובה כל "));
line.append($("<b>").text(viewsCommentsRatio + " צפיות"));
line.append($("<span>").text("."));
$("#forumStats").append(line);


//shorten the words dictionary
var shortWordsDict = [];
for (var i = 0; i < wordsDict.length && wordsDict[i].count > 1; i++)
{
    shortWordsDict.push(wordsDict[i]);
}
wordsDict = shortWordsDict;

//button for detailed statistics
$("#forumStats").append(
    $("<div>", { class: "smallPlusButton", id: "detailedStatsBtn" }).text("+").click(function ()
    {

        var pContent = $("<div>");

        pContent.append($("<div>").text("להלן פירוט הסטטיסטיקות לפורום זה:"));

        var flexTableContainer = $("<div>", { style: "display: flex; flex-wrap: wrap;" });

        //add table skeleton
        flexTableContainer.append($("<table>", { class: "statTable", id: "publishersStatTable" }).append(
            $("<tr>").append(
                $("<th>").text("מפרסם")
            ).append(
                $("<th>").text("אשכולות")
            ))
        );

        flexTableContainer.append($("<table>", { class: "statTable", id: "commentorsStatTable" }).append(
            $("<tr>").append(
                $("<th>").text("מגיב")
            ).append(
                $("<th>").text("תגובות אחרונות")
            ))
        );

        flexTableContainer.append($("<table>", { class: "statTable", id: "wordsStatTable" }).append(
            $("<tr>").append(
                $("<th>").text("מילה")
            ).append(
                $("<th>").text("אזכורים")
            ))
        );

        flexTableContainer.append($("<table>", { class: "statTable", id: "prefixesStatTable" }).append(
            $("<tr>").append(
                $("<th>").text("תיוג")
            ).append(
                $("<th>").text("אשכולות")
            ))
        );

        //add table content
        for (var i = 0; i < publishersDict.length; i++)
        {
            flexTableContainer.find("#publishersStatTable").append(
                $("<tr>").append(
                    $("<td>").text(publishersDict[i].value)
                ).append(
                    $("<td>").text(publishersDict[i].count)
                )
            );
        }

        for (var i = 0; i < commentorsDict.length; i++)
        {
            flexTableContainer.find("#commentorsStatTable").append(
                $("<tr>").append(
                    $("<td>").text(commentorsDict[i].value)
                ).append(
                    $("<td>").text(commentorsDict[i].count)
                )
            );
        }

        for (var i = 0; i < wordsDict.length; i++)
        {
            flexTableContainer.find("#wordsStatTable").append(
                $("<tr>").append(
                    $("<td>").text(wordsDict[i].value)
                ).append(
                    $("<td>").text(wordsDict[i].count)
                )
            );
        }

        for (var i = 0; i < prefixesDict.length; i++)
        {
            flexTableContainer.find("#prefixesStatTable").append(
                $("<tr>").append(
                    $("<td>").text(prefixesDict[i].value)
                ).append(
                    $("<td>").text(prefixesDict[i].count)
                )
            );
        }

        //append all the tables in their container
        pContent.append(flexTableContainer);

        pContent.append(
            $("<div>", { class: "closeBtn" }).text("סגור").click(function ()
            {
                removePopupWindow("detailedStats");
            })
        );

        //open the window
        openPopupWindow("detailedStats",
            "images/graph.svg",
            "סטטיסטיקות מפורטות לפורום " + getForumName(),
            pContent);
    }
    ));
}
    function getDupeSortedDictionary(arr)
    {
        var counts = {}; //count each one
        for (var i = 0; i < arr.length; i++)
        {
            counts[arr[i]] = (counts[arr[i]] || 0) + 1;
        }
    
        var sortedArr = []; //add all properties to an array
        for (var prop in counts)
        {
            sortedArr.push({ value: prop, count: counts[prop] });
        }
        sortedArr.sort(function (a, b) //sort the array according to the counts or alphabetically
        {
            if (b.count === a.count)
            {
                if (b.value < a.value)
                    return 1;
                else if (b.value > a.value)
                    return -1;
                else
                    return 0;
            }
            else
                return b.count - a.count;
        });
        return sortedArr;
    }
    
    function openPopupWindow(id, img, title, content, additionalClass)
{
    if ($("#popupBox").length === 0)
    {
        $("body").append($("<div>", { id: "popupBox" }));
    }

    //do not open a popup if one already exists
    if ($(".popupContainer").length === 0)
    {
        $("body").append($("<div>", { class: "dimScreen", id: "dim_" + id }).click(function ()
        {
            removePopupWindow(id);
        }));

        var popup =
            $("<div>", { class: "popupContainer", id: id }).append(
                $("<div>", { class: "popupTop" }).append(
                    $("<div>", { class: "popupImg" }).append(
                        $("<img>", { src: img })
                    )
                ).append(
                    $("<div>", { class: "popupTitle" }).append(title)
                )
            ).append(
                $("<div>", { class: "popupBottom" }).append(content)
            );

        if (additionalClass)
            popup.addClass(additionalClass);

        $("#popupBox").append(popup);

        $("#dim_" + id).fadeIn(300, function ()
        {
            $("#" + id).show();
        });
    }
}

//removes completely a popup window
function removePopupWindow(id)
{
    if ($("#" + id).length > 0)
    {
        $("#" + id).remove();
        $("#dim_" + id).fadeOut(300, function ()
        {
            $(this).remove();
        });
        return true;
    }
    return false;
}

function getForumName()
{
    var name = $(".pagetitle .ntitle").text().trim(); //try by forum thread list title
    if (name === "")
    {
        $("#breadcrumb .navbit a[href*='?f=']:last").text().trim(); //try by breadcrumb navigation tree
    }
    return name;
}
function httpGetAsync(theUrl, callback)
{
    var xmlHttp = new XMLHttpRequest();
    xmlHttp.onreadystatechange = function ()
    {
        if (xmlHttp.readyState === 4 && xmlHttp.status === 200)
            callback(xmlHttp.responseText);
    };
    xmlHttp.open("GET", theUrl, true); // true for asynchronous 
    xmlHttp.send(null);
}
