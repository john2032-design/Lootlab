const express = require(‘express’)
const fs = require(‘fs’)
const path = require(‘path’)
const { URL } = require(‘url’)
const app = express()
app.use(express.json())
const STORE_PATH = path.resolve(__dirname, ‘redirects.json’)
function loadRedirectMap() {
try {
if (!fs.existsSync(STORE_PATH)) return {}
const raw = fs.readFileSync(STORE_PATH, ‘utf8’)
if (!raw) return {}
return JSON.parse(raw)
} catch (e) {
return {}
}
}
function saveRedirectMap(map) {
try {
fs.writeFileSync(STORE_PATH, JSON.stringify(map, null, 2), ‘utf8’)
return true
} catch (e) {
return false
}
}
function saveRedirectMapping(lootUrl, destUrl) {
try {
const map = loadRedirectMap()
const key = encodeURIComponent(lootUrl)
map[key] = destUrl
saveRedirectMap(map)
return true
} catch (e) {
return false
}
}
function getRedirectFor(lootUrl) {
try {
const map = loadRedirectMap()
const key = encodeURIComponent(lootUrl)
return map[key] || null
} catch (e) {
return null
}
}
function decodeURIxor(encodedString, prefixLength = 5) {
const buf = Buffer.from(encodedString, ‘base64’)
const base64Decoded = buf.toString(‘binary’)
const prefix = base64Decoded.substring(0, prefixLength)
const encodedPortion = base64Decoded.substring(prefixLength)
let decodedString = ‘’
for (let i = 0; i < encodedPortion.length; i++) {
const encodedChar = encodedPortion.charCodeAt(i)
const prefixChar = prefix.charCodeAt(i % prefix.length)
const decodedChar = encodedChar ^ prefixChar
decodedString += String.fromCharCode(decodedChar)
}
return decodedString
}
function tryExtractHrefFromHtml(html, baseUrl) {
const hrefRegex = /<a\s[^>]href=([”’])(.?)\1/gi
let match
while ((match = hrefRegex.exec(html)) !== null) {
const href = match[2].trim()
if (!href) continue
try {
const candidate = new URL(href, baseUrl).toString()
return candidate
} catch (e) {
continue
}
}
return null
}
function tryFindEncodedCandidate(html) {
const base64Regex = /([A-Za-z0-9+/=]{30,})/g
let match
while ((match = base64Regex.exec(html)) !== null) {
const candidate = match[1]
try {
return candidate
} catch (e) {
continue
}
}
return null
}
async function fetchUrl(url) {
if (typeof fetch === ‘undefined’) {
const nodeFetch = await import(‘node-fetch’).then(m => m.default).catch(()=>null)
if (!nodeFetch) throw new Error(‘fetch not available’)
return nodeFetch(url, { redirect: ‘follow’ })
} else {
return fetch(url, { redirect: ‘follow’ })
}
}
app.get(’/bypass’, async (req, res) => {
const start = process.hrtime.bigint()
const queryUrl = (req.query && req.query.url) ? req.query.url : null
if (!queryUrl) {
const elapsed = Number(process.hrtime.bigint() - start) / 1e9
res.json({ result: null, Time: elapsed.toFixed(2), Status: ‘Error’ })
return
}
try {
const saved = getRedirectFor(queryUrl)
if (saved) {
const elapsed = Number(process.hrtime.bigint() - start) / 1e9
res.json({ result: saved, Time: elapsed.toFixed(2), Status: ‘Success’ })
return
}
let finalUrl = null
try {
const response = await fetchUrl(queryUrl)
const contentType = response.headers && response.headers.get ? (response.headers.get(‘content-type’) || ‘’) : ‘’
if (response.ok && contentType.includes(‘text’)) {
const html = await response.text()
const href = tryExtractHrefFromHtml(html, queryUrl)
if (href) finalUrl = href
if (!finalUrl) {
const encoded = tryFindEncodedCandidate(html)
if (encoded) {
try {
const decoded = decodeURIxor(encoded)
try {
const decodedUrl = decodeURIComponent(decoded)
if (decodedUrl && (decodedUrl.startsWith(‘http://’) || decodedUrl.startsWith(‘https://’))) finalUrl = decodedUrl
else {
try {
const maybe = new URL(decoded, queryUrl).toString()
finalUrl = maybe
} catch (e) {}
}
} catch (e) {
if (decoded && (decoded.startsWith(‘http://’) || decoded.startsWith(‘https://’))) finalUrl = decoded
}
} catch (e) {}
}
}
} else if (response.ok && contentType.includes(‘application/json’)) {
const json = await response.json().catch(()=>null)
if (json) {
if (Array.isArray(json)) {
for (const item of json) {
if (item && item.urid) {
const candidate = item.PUBLISHER_LINK || item.publisher_link || item.link || item.url || null
if (candidate) {
finalUrl = candidate
break
}
}
}
} else {
const candidate = json.PUBLISHER_LINK || json.publisher_link || json.link || json.url || null
if (candidate) finalUrl = candidate
}
}
} else {
try {
const redirected = response.url
if (redirected && redirected !== queryUrl) finalUrl = redirected
} catch (e) {}
}
} catch (e) {}
if (!finalUrl) {
const elapsed = Number(process.hrtime.bigint() - start) / 1e9
res.json({ result: null, Time: elapsed.toFixed(2), Status: ‘Error’ })
return
}
try {
saveRedirectMapping(queryUrl, finalUrl)
} catch (e) {}
const elapsed = Number(process.hrtime.bigint() - start) / 1e9
res.json({ result: finalUrl, Time: elapsed.toFixed(2), Status: ‘Success’ })
return
} catch (e) {
const elapsed = Number(process.hrtime.bigint() - start) / 1e9
res.json({ result: null, Time: elapsed.toFixed(2), Status: ‘Error’ })
return
}
})
const port = process.env.PORT || 3000
app.listen(port, () => {})
