var bookmarks = {
    "Google": "https://google.com",
    "Wikipedia": "https://www.wikipedia.org/",
    "Youtube": "https://www.youtube.com/"
};

var nameAlertQueued = false;
var urlAlertQueued = false;
var emptyNameAlertQueued = false;
var emptyUrlAlertQueued = false;


function validName(str) {
    return !(str in bookmarks);
}

function validURL(str) {
    var pattern = new RegExp('^(https?:\\/\\/)?' +
        '((([a-z\\d]([a-z\\d-]*[a-z\\d])*)\\.)+[a-z]{2,}|' +
        '((\\d{1,3}\\.){3}\\d{1,3}))' +
        '(\\:\\d+)?(\\/[-a-z\\d%_.~+]*)*' +
        '(\\?[;&a-z\\d%_.~+=-]*)?' +
        '(\\#[-a-z\\d_]*)?$', 'i');
    return !!pattern.test(str);
}

function clearStorage() {
    localStorage.removeItem("populated");
    localStorage.removeItem("bookmarks");
    console.log("Deleted Storage. Please Refresh Page.");
}

function populateStorage() {
    localStorage.setItem("populated", "true");
    console.log("Populated Storage.");
    writeStorage();
}

function readStorage() {
    bookmarks = JSON.parse(localStorage.getItem("bookmarks"));
    console.log("Read Storage.");
}

function writeStorage() {
    localStorage.setItem("bookmarks", JSON.stringify(bookmarks));
    console.log("Written Storage.");
}

function addBookmark(name, url) {
    if (!validURL(url))
        throw "Invalid URL.";
    bookmarks[name] = url;
    writeStorage();
}

function removeBookmark(name) {
    delete bookmarks[name];
    writeStorage();
}

function getBookmarks() {
    return bookmarks
}

function craftBookmark(name, url) {
    return `<a onclick="open_link(this);" class="list-group-item list-group-item-action clearfix"><div class="link text-muted"><span class="h5 align-middle text-body">${name}<br/></span><span>${url}</span> </div><span class="buttons align-middle float-right"> <button onclick="event.stopPropagation();remove(this.parentNode.parentNode);" class="btn btn-sm btn-danger">Delete</button></span></a>`;
}

function updatePage() {
    var html = '';
    for (var bookmark in bookmarks)
        html += craftBookmark(bookmark, bookmarks[bookmark]);
    if (html.length == 0)
        html = `<p class="pt-3 lead text-light">No Bookmarks.</p>`;
    $("#bookmarks").html(html);
    console.log("Updated Page.");
}

function identify(tag) {
    return $(tag).children().children().html().slice(0, -4);  // dirty, but works
}

function open_link(tag) {
    var key = identify(tag);
    window.open(bookmarks[key]);
    console.log("Opened Link.");
}

function remove(tag) {
    var key = identify(tag);
    tag.parentNode.removeChild(tag);
    removeBookmark(key);
    updatePage();
}

function alertEmptyName() {
    if (!Boolean($("#emptyName").length)) {
        $("#alerts").html($("#alerts").html() + `<div id="emptyName" class="alert alert-danger" role="alert">There's no name specified for the new bookmark. <button type="button" class="close" data-dismiss="alert" aria-label="Close"> <span aria-hidden="true">&times;</span> </button> </div>`);
    }
    else
        emptyNameAlertQueued = true;
    setTimeout(function () {
        if (emptyNameAlertQueued)
            emptyNameAlertQueued = false;
        else
            $("#emptyName").alert('close');
    }, 3000);
}

function alertEmptyURL() {
    if (!Boolean($("#emptyURL").length)) {
        $("#alerts").html($("#alerts").html() + `<div id="emptyURL" class="alert alert-danger" role="alert">There's no URL specified for the new bookmark. <button type="button" class="close" data-dismiss="alert" aria-label="Close"> <span aria-hidden="true">&times;</span> </button> </div>`);
    }
    else
        emptyUrlAlertQueued = true;
    setTimeout(function () {
        if (emptyUrlAlertQueued)
            emptyUrlAlertQueued = false;
        else
            $("#emptyURL").alert('close');
    }, 3000);
}

function alertInvalidName() {
    if (!Boolean($("#invalidName").length)) {
        $("#alerts").html($("#alerts").html() + `<div id="invalidName" class="alert alert-warning" role="alert">There's already a bookmark with that name. <button type="button" class="close" data-dismiss="alert" aria-label="Close"> <span aria-hidden="true">&times;</span> </button> </div>`);
    }
    else
        nameAlertQueued = true;
    setTimeout(function () {
        if (nameAlertQueued)
            nameAlertQueued = false;
        else
            $("#invalidName").alert('close');
    }, 3000);
}

function alertInvalidURL() {
    if (!Boolean($("#invalidURL").length)) {
        $("#alerts").html($("#alerts").html() + `<div id="invalidURL" class="alert alert-danger" role="alert">That's not a valid URL. <button type="button" class="close" data-dismiss="alert" aria-label="Close"> <span aria-hidden="true">&times;</span> </button> </div>`);
    }
    else
        urlAlertQueued = true;
    setTimeout(function () {
        if (urlAlertQueued)
            urlAlertQueued = false;
        else
            $("#invalidURL").alert('close');
    }, 3000);
}

function deleteAlerts() {
    $("#emptyName").alert('close');
    $("#emptyURL").alert('close');
    $("#invalidName").alert('close');
    $("#invalidURL").alert('close');
}

function emptyFields() {
    $("#name").val('');
    $("#url").val('');
}

function create() {
    name = $("#name").val();
    url = $("#url").val();

    if (name.length == 0)
        alertEmptyName();
    if (url.length == 0)
        alertEmptyURL();

    if (name.length == 0 || url.length == 0)
        return undefined;

    if (!validName(name))
        alertInvalidName();
    if (!validURL(url))
        alertInvalidURL();

    if (!validName(name) || !validURL(url))
        return undefined;

    addBookmark(name, url);
    updatePage();

    deleteAlerts();
    emptyFields();
}

if (!localStorage.getItem("populated"))
    populateStorage();
else
    readStorage();

updatePage();
