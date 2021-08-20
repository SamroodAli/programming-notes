// get request

async function getNewMsgs() {
  // poll the server
  // write code here
  let json;
  try {
    const res = await fetch("/poll");
    json = await res.json();
  } catch (e) {
    //backoff code
    console.error("Polling error", e);
  }
  allChat = json.msg;
  render();
  setTimeout(getNewMsgs, INTERVAL);
}

// lose focus get requester
async function rafTimer(time) {
  if (timeToMakeNextRequest <= time) {
    await getNewMsgs();
    timeToMakeNextRequest = time + INTERVAL;
  }
  requestAnimationFrame(rafTime);
}

requestAnimationFrame(rafTimer);
//rafTimer calls the callback with the time since the browser session (got the page) started in ms

// post request

async function getNewMsgs() {
  // poll the server
  // write code here
  let json;
  try {
    const res = await fetch("/poll");
    json = await res.json();
  } catch (e) {
    //backoff code
    console.error("Polling error", e);
  }
  allChat = json.msg;
  render();
  setTimeout(getNewMsgs, INTERVAL);
}


// how to stop on unfocus
