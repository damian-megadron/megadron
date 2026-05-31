// ==UserScript==
// @name         Megadron Scripts Packed
// @namespace    megadron.pl
// @version      1.0
// @description  do repozytorium
// @author       DF
// @match        https://client6220.idosell.com/*
// @match        https://megadron.pl/pl/products/*
// @grant        GM_xmlhttpRequest
// @connect      client6220.idosell.com
// ==/UserScript==

/* global $, IAI */ //bo krzyczy

(function () {
    'use strict';

    const url = window.location.href;


    // GUZICZKI GABARYTÓW INPOST

    if (url.includes('/panel/order-verification.php')) {

        window.addEventListener("load", () => {
            addButtonsGabaryt();
        });

        function addButtonsGabaryt() {
            var scriptDelivery = document.getElementById("deliverer-number-count_" + IAI._orderNumber);
            scriptDelivery = scriptDelivery.getAttribute("onclick");
            scriptDelivery = scriptDelivery.replace('show_packages_edit_toplayer(event,', '');
            scriptDelivery = scriptDelivery.replace(')', '');
            var jsonObject = JSON.parse(scriptDelivery);
            var packageId = jsonObject.delivery_packages.packs[0].id;

            if (!jsonObject.delivery_packages.packs[0].package_number) {
                var mydiv, newDiv;
                if (document.querySelector('[title="Paczkomaty InPost Smile"]')) {
                    mydiv = document.querySelector("#tr_0 > td:nth-child(5)");
                    newDiv = "<br/><div id=\"wyborGabarytu\"><input type=\"button\" value=\"Gabaryt A\" onclick=\"IAI.ajax.post('/panel/ajax/orderd.php', '', 'action=assignBoxToPackage&packageId=" + packageId + "&boxType=A'); IAI.ajax.post('/panel/ajax/packages-gateway.php', '', 'courierId=" + jsonObject.deliverer_id + "&action=generate&orders[]=" + jsonObject.order_id + "'); $('#wyborGabarytu').html('<br/><b>Przypisano gabaryt A!</b>'); document.querySelector('#fg_code').focus();\" style=\"margin-top:20px; height:50px; font-size:15px; border-radius: 3px\" class=\"btn btn-sm btn-success formbutton\">";
                    newDiv += "<input type=\"button\" value=\"Gabaryt B\" onclick=\"IAI.ajax.post('/panel/ajax/orderd.php', '', 'action=assignBoxToPackage&packageId=" + packageId + "&boxType=B'); IAI.ajax.post('/panel/ajax/packages-gateway.php', '', 'courierId=" + jsonObject.deliverer_id + "&action=generate&orders[]=" + jsonObject.order_id + "'); $('#wyborGabarytu').html('<br/><b>Przypisano gabaryt B!</b>'); document.querySelector('#fg_code').focus();\" style=\"margin-top:20px; height:50px; font-size:15px; border-radius: 3px\" class=\"btn btn-sm btn-warning formbutton\">";
                    newDiv += "<input type=\"button\" value=\"Gabaryt C\" onclick=\"IAI.ajax.post('/panel/ajax/orderd.php', '', 'action=assignBoxToPackage&packageId=" + packageId + "&boxType=C'); IAI.ajax.post('/panel/ajax/packages-gateway.php', '', 'courierId=" + jsonObject.deliverer_id + "&action=generate&orders[]=" + jsonObject.order_id + "'); $('#wyborGabarytu').html('<br/><b>Przypisano gabaryt C!</b>'); document.querySelector('#fg_code').focus();\" style=\"margin-top:20px; height:50px; font-size:15px; border-radius: 3px\" class=\"btn btn-sm btn-danger formbutton\"></div>";
                    mydiv.innerHTML += newDiv;
                } else if (document.querySelector('[title="Allegro Paczkomaty 24/7 InPost - wszystkie rozmiary"]') || document.querySelector('[title="InPost Paczkomaty 24/7 - wszystkie rozmiary"]')) {
                    mydiv = document.querySelector("#tr_0 > td:nth-child(5)");
                    newDiv = "<br/><div id=\"wyborGabarytu\"><input type=\"button\" value=\"Gabaryt A\" onclick=\"IAI.ajax.post('/panel/ajax/packages-gateway.php', '', 'defaultParcelGauge=small&courierId=" + jsonObject.deliverer_id + "&action=generate&orders[]=" + jsonObject.order_id + "'); $('#wyborGabarytu').html('<br/><b>Przypisano gabaryt A!</b>'); document.querySelector('#fg_code').focus();\" style=\"margin-top:20px; height:50px; font-size:15px; border-radius: 3px\" class=\"btn btn-sm btn-success formbutton\">";
                    newDiv += "<input type=\"button\" value=\"Gabaryt B\" onclick=\"IAI.ajax.post('/panel/ajax/packages-gateway.php', '', 'defaultParcelGauge=medium&courierId=" + jsonObject.deliverer_id + "&action=generate&orders[]=" + jsonObject.order_id + "'); $('#wyborGabarytu').html('<br/><b>Przypisano gabaryt B!</b>'); document.querySelector('#fg_code').focus();\" style=\"margin-top:20px; height:50px; font-size:15px; border-radius: 3px\" class=\"btn btn-sm btn-warning formbutton\">";
                    newDiv += "<input type=\"button\" value=\"Gabaryt C\" onclick=\"IAI.ajax.post('/panel/ajax/packages-gateway.php', '', 'defaultParcelGauge=large&courierId=" + jsonObject.deliverer_id + "&action=generate&orders[]=" + jsonObject.order_id + "'); $('#wyborGabarytu').html('<br/><b>Przypisano gabaryt C!</b>'); document.querySelector('#fg_code').focus();\" style=\"margin-top:20px; height:50px; font-size:15px; border-radius: 3px\" class=\"btn btn-sm btn-danger formbutton\">";
                    mydiv.innerHTML += newDiv;
                } else {
                    IAI.ajax.post('/panel/ajax/packages-gateway.php', '', 'courierId=' + jsonObject.deliverer_id + '&action=generate&orders[]=' + jsonObject.order_id);
                }
            }
            var badDivs = $("#msg_line_msg");
            badDivs.remove();
        }
    }


    // ════════════════════════════════════════════════════════════════════════
    // STAN M1 PRZY PAKOWANIU
    // ════════════════════════════════════════════════════════════════════════

    if (url.includes('/panel/order-verification.php')) {

        function fetchM1(idt, callback) {
            GM_xmlhttpRequest({
                method: 'POST',
                url: `https://client6220.idosell.com/panel/ajax/product-edit-aceform-tables.php?name=quantity&idt=${idt}&filtered=false`,
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
                    'X-Requested-With': 'XMLHttpRequest',
                    'Referer': `https://client6220.idosell.com/panel/product-edit.php?idt=${idt}`
                },
                data: `id=stocksQuantity&url=/panel/ajax/product-edit-aceform-tables.php%3Fname%3Dquantity%26idt%3D${idt}%26filtered%3Dfalse`,
                onload: function (response) {
                    try {
                        const parser = new DOMParser();
                        const doc = parser.parseFromString(response.responseText, 'text/html');
                        const allRows = doc.querySelectorAll('table tbody tr');
                        let row = null;
                        allRows.forEach(r => {
                            const cells = r.querySelectorAll('td');
                            if (!row && cells.length >= 2 && cells[1].textContent.trim() !== '' && cells[1].textContent.trim() !== '-') {
                                row = r;
                            }
                        });
                        if (!row) { callback(null); return; }
                        const cells = row.querySelectorAll('td');
                        callback(cells[1] ? cells[1].textContent.trim() : null);
                    } catch (e) {
                        callback(null);
                    }
                },
                onerror: function () { callback(null); }
            });
        }

        function makeBadgeM1(m1) {
            const qty = parseInt(m1, 10);
            let bg, border, color;
            if (m1 === null) {
                bg = '#f8d7da'; border = '#dc3545'; color = '#721c24';
            } else if (isNaN(qty) || qty <= 0) {
                bg = '#f8d7da'; border = '#dc3545'; color = '#721c24';
            } else if (qty <= 2) {
                bg = '#fff3cd'; border = '#ffc107'; color = '#856404';
            } else {
                bg = '#d4edda'; border = '#28a745'; color = '#155724';
            }
            const d = document.createElement('div');
            d.className = 'm1-stock-badge';
            d.style.cssText = `
            margin-top:4px;
            padding:2px 8px;
            border-radius:4px;
            font-size:12px;
            display:inline-block;
            background:${bg};
            border:1px solid ${border};
            color:${color};
            `;

            d.innerHTML = `📦 M1: <strong>${m1 !== null ? m1 : '-'}</strong>`;
            return d;
        }

        function processLinksM1() {
            document.querySelectorAll('a[onclick*="showProductInfo"]').forEach(a => {
                if (a.dataset.m1processed) return;
                a.dataset.m1processed = '1';

                const match = a.getAttribute('onclick').match(/showProductInfo\(`(\d+)`/);
                if (!match) return;
                const idt = match[1];

                const tr = a.closest('tr');
                if (!tr) return;

                const nameTd = tr.querySelector('td[class*="col-name"]');
                if (!nameTd) return;

                let lokalizacjeP = null;
                nameTd.querySelectorAll('p').forEach(p => {
                    if (!lokalizacjeP && p.textContent.includes('Lokalizacje:')) {
                        lokalizacjeP = p;
                    }
                });
                if (!lokalizacjeP) return;
                if (lokalizacjeP.parentElement.querySelector('.m1-stock-badge')) return;

                const placeholder = document.createElement('div');
                placeholder.className = 'm1-stock-badge';
                placeholder.style.cssText = `margin-top:4px;padding:2px 8px;border-radius:4px;font-size:12px;display:inline-block;background:#fff3cd;border:1px solid #ffc107;color:#856404;`;
                placeholder.innerHTML = '📦 M1: <strong>ładowanie…</strong>';
                lokalizacjeP.insertAdjacentElement('afterend', placeholder);

                fetchM1(idt, function (m1) {
                    placeholder.replaceWith(makeBadgeM1(m1));
                });
            });
        }

        const observerM1 = new MutationObserver(processLinksM1);
        observerM1.observe(document.body, { childList: true, subtree: true });
        processLinksM1();
    }


    // ════════════════════════════════════════════════════════════════════════
    // UKRYJ WIERSZE W HISTORII ZAMÓWIENIA
    // ════════════════════════════════════════════════════════════════════════

    if (url.includes('/panel/orderd.php') || url.includes('/panel/app/orderd.php')) {

        const NEEDLE_PHRASES = [
            'Nie udało się wykonać automatycznej zmiany statusu zamówienia, gdy wszystkie towary znalazły się na magazynach własnych. Niewystarczająca ilość towaru na stanie dyspozycyjnym'
        ];
        const MARK_ATTR = 'data-tm-hidden-by-script';
        let hiddenCount = 0;
        let summaryEl = null;
        let toggleBtn = null;
        let showingHidden = false;

        function shouldHideRowText(text) {
            const t = (text || '').trim();
            return NEEDLE_PHRASES.some(p => t.includes(p));
        }

        function insertUIAfterAnchor() {
            const anchor = document.querySelector('div.hd#orderHistoryDiv_h');
            if (!anchor || !anchor.parentNode) return false;
            if (document.getElementById('tm-hidden-rows-summary')) return true;

            summaryEl = document.createElement('div');
            summaryEl.id = 'tm-hidden-rows-summary';
            summaryEl.textContent = 'Skrypt ukrył 0 wierszy';
            summaryEl.style.cssText =`
            margin:12px 0 8px 0;
            padding:10px 12px;
            border:2px solid #d00;
            color:#d00;
            font-weight:700;
            background:#fff5f5;
            font-family:Arial,sans-serif;
            font-size:14px;';
            `

            toggleBtn = document.createElement('button');
            toggleBtn.type = 'button';
            toggleBtn.id = 'tm-toggle-hidden-rows';
            toggleBtn.textContent = 'Pokaż ukryte';
            toggleBtn.style.cssText = 'margin:0 0 12px 0;padding:6px 12px;cursor:pointer;font-weight:700;';

            toggleBtn.addEventListener('click', () => {
                showingHidden = !showingHidden;
                document.querySelectorAll(`tr[${MARK_ATTR}="1"]`).forEach(tr => {
                    tr.style.display = showingHidden ? '' : 'none';
                });
                toggleBtn.textContent = showingHidden ? 'Ukryj ponownie' : 'Pokaż ukryte';
            });

            anchor.parentNode.insertBefore(summaryEl, anchor.nextSibling);
            anchor.parentNode.insertBefore(toggleBtn, summaryEl.nextSibling);
            return true;
        }

        function ensureUI() { insertUIAfterAnchor(); }

        function updateSummaryHistory() {
            ensureUI();
            const el = document.getElementById('tm-hidden-rows-summary');
            if (el) el.textContent = `Skrypt ukrył ${hiddenCount} wierszy`;
        }

        function processRowsHistory(root = document) {
            const rows = root.querySelectorAll ? root.querySelectorAll('tr') : [];
            for (const tr of rows) {
                if (!(tr instanceof HTMLElement)) continue;
                if (tr.hasAttribute(MARK_ATTR)) continue;
                if (shouldHideRowText(tr.textContent || '')) {
                    tr.setAttribute(MARK_ATTR, '1');
                    tr.style.display = showingHidden ? '' : 'none';
                    hiddenCount += 1;
                } else {
                    tr.setAttribute(MARK_ATTR, '0');
                }
            }
            updateSummaryHistory();
        }

        function scheduleRescanSequence() {
            [0, 200, 600, 1200, 2000].forEach(d => setTimeout(() => processRowsHistory(document), d));
        }

        function hookHistoryClick() {
            document.addEventListener('click', (e) => {
                const a = e.target && e.target.closest ? e.target.closest('a') : null;
                if (!a) return;
                const onclick = a.getAttribute('onclick') || '';
                const txt = (a.textContent || '').trim();
                if (txt === 'Historia zamówienia' || onclick.includes('IAI._orderd.orderHistory(')) {
                    scheduleRescanSequence();
                }
            }, true);
        }

        function observeDomHistory() {
            const obs = new MutationObserver(mutations => {
                for (const m of mutations) {
                    for (const node of m.addedNodes) {
                        if (!(node instanceof HTMLElement)) continue;
                        if (node.matches?.('div.hd#orderHistoryDiv_h') || node.querySelector?.('div.hd#orderHistoryDiv_h')) {
                            ensureUI();
                        }
                        processRowsHistory(node);
                    }
                }
            });
            obs.observe(document.body, { childList: true, subtree: true });
        }

        function startHistory() {
            ensureUI();
            processRowsHistory(document);
            hookHistoryClick();
            observeDomHistory();
        }

        if (document.readyState === 'loading') {
            document.addEventListener('DOMContentLoaded', startHistory);
        } else {
            startHistory();
        }
    }


    // ════════════════════════════════════════════════════════════════════════
    // PODGLĄD DŁUŻSZYCH NOTATEK
    // ════════════════════════════════════════════════════════════════════════

    if (url.includes('client6220.idosell.com')) {

        let tooltipNote;
        const MIN_LENGTH = 100;

        function createTooltip() {
            tooltipNote = document.createElement('div');
            tooltipNote.style.position = 'fixed';
            tooltipNote.style.zIndex = '99999';
            tooltipNote.style.top = '50%';
            tooltipNote.style.right = '20px';
            tooltipNote.style.transform = 'translateY(-50%)';
            tooltipNote.style.maxWidth = '420px';
            tooltipNote.style.padding = '10px 12px';
            tooltipNote.style.background = '#ffffff';
            tooltipNote.style.border = '1px solid #ccc';
            tooltipNote.style.borderRadius = '8px';
            tooltipNote.style.boxShadow = '0 6px 18px rgba(0,0,0,.18)';
            tooltipNote.style.fontSize = '13px';
            tooltipNote.style.fontWeight = '700';
            tooltipNote.style.lineHeight = '1.45';
            tooltipNote.style.color = 'red';
            tooltipNote.style.whiteSpace = 'pre-wrap';
            tooltipNote.style.display = 'none';
            document.body.appendChild(tooltipNote);
        }

        function showTooltipNote(text) {
            if (!tooltipNote) createTooltip();
            tooltipNote.textContent = text;
            tooltipNote.style.display = 'block';
        }

        function hideTooltipNote() {
            if (tooltipNote) tooltipNote.style.display = 'none';
        }

        function initTooltip() {
            document.querySelectorAll('a[data-function_delegate="TextareaEdit"]').forEach(el => {
                if (el.dataset.tmTooltipInit) return;
                el.dataset.tmTooltipInit = '1';
                const text = (el.getAttribute('title') || el.textContent || '').trim();
                if (text.length <= MIN_LENGTH) return;
                el.addEventListener('mouseenter', () => showTooltipNote(text));
                el.addEventListener('mouseleave', hideTooltipNote);
            });
        }

        initTooltip();
        new MutationObserver(initTooltip).observe(document.body, { childList: true, subtree: true });
    }


    // ════════════════════════════════════════════════════════════════════════
    // ALLEGRO OPISY KURIERÓW
    // ════════════════════════════════════════════════════════════════════════

    if (url.includes('client6220.idosell.com')) {

        const allegroCourierMap = [
            { match: /orlen/i, label: 'Orlen Paczka' },
            { match: /\bone\b/i, label: 'One Kurier' },
            { match: /inpost/i, label: 'InPost' },
            { match: /dpd/i, label: 'DPD' },
            { match: /dhl/i, label: 'DHL' },
            { match: /ups/i, label: 'UPS' },
            { match: /gls/i, label: 'GLS' },
            { match: /pocztex/i, label: 'Pocztex' },
            { match: /poczta|pp/i, label: 'PP' }
        ];

        function getCourierLabel(name) {
            for (const courier of allegroCourierMap) {
                if (courier.match.test(name)) return courier.label;
            }
            return null;
        }

        function addCourierLabels() {
            document.querySelectorAll('img.delivery-icon').forEach(img => {
                const parent = img.parentElement;
                if (!parent) return;
                if (parent.querySelector('.tm-courier-label')) return;
                const fullName = img.title || img.alt;
                if (!fullName) return;
                if (!/allegro/i.test(fullName)) return;
                const shortName = getCourierLabel(fullName);
                if (!shortName) return;

                parent.style.textAlign = 'center';
                const label = document.createElement('div');
                label.className = 'tm-courier-label';
                label.textContent = shortName;
                label.style.fontSize = '12px';
                label.style.fontWeight = '600';
                label.style.lineHeight = '1.3';
                label.style.marginTop = '4px';
                label.style.color = '#333';
                label.style.whiteSpace = 'normal';
                label.style.wordBreak = 'break-word';
                label.style.maxWidth = '70px';
                parent.appendChild(label);
            });
        }

        addCourierLabels();
        new MutationObserver(addCourierLabels).observe(document.body, { childList: true, subtree: true });
    }


    // ════════════════════════════════════════════════════════════════════════
    // SN-y WIĘKSZY PRZYCISK
    // ════════════════════════════════════════════════════════════════════════

    if (url.includes('/panel/order-verification.php')) {

        const STYLE_ID = 'serial-button-style';

        function addStylesSN() {
            if (document.getElementById(STYLE_ID)) return;
            const style = document.createElement('style');
            style.id = STYLE_ID;
            style.textContent = `
                a.serial-button {
                    display: inline-block !important;
                    padding: 8px 14px !important;
                    background-color: #428bca !important;
                    color: #fff !important;
                    border-radius: 4px !important;
                    font-weight: 600 !important;
                    text-decoration: none !important;
                    cursor: pointer !important;
                    border: 1px solid #1a68d1 !important;
                }
                a.serial-button:hover {
                    background-color: #3b7db6 !important;
                    color: #fff !important;
                    text-decoration: none !important;
                }
            `;
            document.head.appendChild(style);
        }

        function convertLinks() {
            document.querySelectorAll('a').forEach(a => {
                if (a.textContent.trim().toLowerCase() === 'wprowadź numery seryjne') {
                    a.classList.add('serial-button');
                }
            });
        }

        addStylesSN();
        convertLinks();
        new MutationObserver(convertLinks).observe(document.body, { childList: true, subtree: true });
    }


    // ════════════════════════════════════════════════════════════════════════
    // LINKI Z ZAN DO ALLEGRO
    // ════════════════════════════════════════════════════════════════════════

    if (url.includes('/panel/orderd.php') || url.includes('/panel/app/orderd.php')) {

        var allegroUrl = "https://salescenter.allegro.com/orders/";
        var empikUrl = "https://marketplace.empik.com/mmp/shop/order/";

        function detectSource(li) {
            var container = li.closest("fieldset, .box, .panel, .content, .section, form, body") || document.body;
            var img = container.querySelector('img[title="Empik"], img[title="Allegro"]');
            if (img) return (img.getAttribute("title") || "").trim();
            img = document.querySelector('img[title="Empik"], img[title="Allegro"]');
            if (img) return (img.getAttribute("title") || "").trim();
            return "";
        }

        function addAllegroButton(orderId) {
            if (document.getElementById("allegro-order-button")) return;
            var btn = document.createElement("a");
            btn.id = "allegro-order-button";
            btn.href = allegroUrl + orderId;
            btn.target = "_blank";
            btn.rel = "noopener noreferrer";
            btn.textContent = "Allegro";
            btn.style.position = "fixed";
            btn.style.right = "16px";
            btn.style.bottom = "16px";
            btn.style.zIndex = "9999";
            btn.style.padding = "10px 16px";
            btn.style.background = "#ff5a00";
            btn.style.color = "#fff";
            btn.style.fontSize = "14px";
            btn.style.fontWeight = "700";
            btn.style.borderRadius = "6px";
            btn.style.textDecoration = "none";
            btn.style.boxShadow = "0 2px 6px rgba(0,0,0,.3)";
            btn.onmouseenter = function () { btn.style.background = "#e64e00"; };
            btn.onmouseleave = function () { btn.style.background = "#ff5a00"; };
            document.body.appendChild(btn);
        }

        function runAllegroEmpik() {
            var listItems = document.getElementsByTagName("li");
            Array.prototype.forEach.call(listItems, function (li) {
                if (li.getAttribute("data-linkdone") === "1") return;
                var span = li.querySelector("span");
                if (!span) return;
                if (span.textContent.trim() !== "ID zamówienia w serwisie zewnętrznym:") return;
                var orderId = li.textContent.replace(span.textContent, "").trim();
                if (!orderId) return;
                var source = detectSource(li);
                var baseUrl = null;
                var label = null;
                if (source === "Empik") {
                    baseUrl = empikUrl;
                    label = "Empik";
                } else if (source === "Allegro") {
                    baseUrl = allegroUrl;
                    label = "Allegro";
                    addAllegroButton(orderId);
                } else {
                    return;
                }
                li.innerHTML = '<span style="font-weight:bold;">ID zamówienia w serwisie zewnętrznym (' + label + '):</span>';
                var a = document.createElement("a");
                a.href = baseUrl + orderId;
                a.textContent = orderId;
                a.target = "_blank";
                a.rel = "noopener noreferrer";
                li.appendChild(a);
                li.setAttribute("data-linkdone", "1");
            });
        }

        runAllegroEmpik();
        setTimeout(runAllegroEmpik, 500);
        setTimeout(runAllegroEmpik, 1500);
    }


    // ════════════════════════════════════════════════════════════════════════
    // INNE ZAMÓWIENIA KLIENTA W REALIZACJI
    // ════════════════════════════════════════════════════════════════════════

    if (url.includes('/panel/orderd.php') || url.includes('/panel/app/orderd.php')) {

        function runOtherOrders() {
            const old = document.getElementById('tm-order-box');
            if (old) old.remove();
            const links = document.querySelectorAll('a[style*="--color-status-on-order"]');
            if (links.length === 0) return;
            const orders = [...links].map(a => ({ number: a.textContent.trim(), href: a.href }));

            const box = document.createElement('div');
            box.id = 'tm-order-box';`
            box.style.cssText = '
            position:fixed;
            top:60px;right:20px;
            z-index:99999;
            background:#c62828;
            color:#fff;
            font-family:Arial,sans-serif;
            font-size:13px;
            font-weight:bold;
            padding:12px 16px;
            border-radius:6px;
            box-shadow:0 4px 16px rgba(0,0,0,0.4);
            max-width:280px;
            line-height:1.6;';
            `

            const title = document.createElement('div');
            title.style.cssText = 'font-size:15px;margin-bottom:8px;';
            title.textContent = `⚠️ Inne zamówienia w realizacji (${orders.length})`;
            box.appendChild(title);

            orders.forEach(o => {
                const row = document.createElement('div');
                const a = document.createElement('a');
                a.href = o.href;
                a.target = '_blank';
                a.textContent = `→ #${o.number}`;
                a.style.cssText = 'color:#fff;text-decoration:underline;';
                row.appendChild(a);
                box.appendChild(row);
            });

            document.body.appendChild(box);
        }

        let debounceOther;
        const observerOther = new MutationObserver(() => {
            clearTimeout(debounceOther);
            debounceOther = setTimeout(runOtherOrders, 600);
        });
        observerOther.observe(document.body, { childList: true, subtree: true });
        setTimeout(() => { observerOther.disconnect(); runOtherOrders(); }, 8000);

        if (document.readyState === 'loading') {
            document.addEventListener('DOMContentLoaded', runOtherOrders);
        } else {
            runOtherOrders();
        }
    }


    // ════════════════════════════════════════════════════════════════════════
    // DUPLIKATY KLIENTÓW HIGHLIGHT
    // ════════════════════════════════════════════════════════════════════════

    if (url.includes('/panel/orders-list.php')) {

        function runDuplicates() {
            const cells = document.querySelectorAll('td.yui-dt0-col-billing_address div[id^="bl"]');
            if (cells.length === 0) return;
            const nameMap = new Map();
            cells.forEach(div => {
                const b = div.querySelector('b');
                if (!b) return;
                const name = b.textContent.trim();
                if (!nameMap.has(name)) nameMap.set(name, []);
                nameMap.get(name).push(b);
            });
            nameMap.forEach((elements, name) => {
                if (elements.length > 1) {
                    elements.forEach(b => {
                        b.style.cssText = `
                        background:#fff176;
                        color:#b71c1c;
                        padding:1px 4px;
                        border-radius:3px;
                        outline:2px solid #e53935;';
                        `
                        b.title = `Ten klient ma ${elements.length} zamówienia na liście`;
                    });

                } else {
                    elements.forEach(b => b.style.cssText = '');
                }
            });
        }

        let debounceDup;
        const observerDup = new MutationObserver(() => {
            clearTimeout(debounceDup);
            debounceDup = setTimeout(runDuplicates, 600);
        });
        observerDup.observe(document.body, { childList: true, subtree: true });
        setTimeout(() => { observerDup.disconnect(); runDuplicates(); }, 8000);

        if (document.readyState === 'loading') {
            document.addEventListener('DOMContentLoaded', runDuplicates);
        } else {
            runDuplicates();
        }
    }


    // ════════════════════════════════════════════════════════════════════════
    // "ZAREJESTROWANE PRZEZ API" NA ZIELONO
    // ════════════════════════════════════════════════════════════════════════

    if (url.includes('/panel/orders-list.php')) {

        const styleApi = document.createElement('style');
        styleApi.textContent = 'tr.tm-api-order td { background-color: #c8e6c9 !important; }';
        document.head.appendChild(styleApi);

        function runApiOrders() {
            document.querySelectorAll('table tbody tr').forEach(tr => {
                const statusCell = tr.querySelector('td.yui-dt0-col-status');
                if (!statusCell) return;
                const hasApi = [...statusCell.querySelectorAll('span')].some(span =>
                    span.textContent.trim() === 'zarejestrowane przez API'
                );
                if (hasApi) {
                    tr.classList.add('tm-api-order');
                } else {
                    tr.classList.remove('tm-api-order');
                }
            });
        }

        let debounceApi;
        const observerApi = new MutationObserver(() => {
            clearTimeout(debounceApi);
            debounceApi = setTimeout(runApiOrders, 600);
        });
        observerApi.observe(document.body, { childList: true, subtree: true });
        setTimeout(() => { observerApi.disconnect(); runApiOrders(); }, 8000);

        if (document.readyState === 'loading') {
            document.addEventListener('DOMContentLoaded', runApiOrders);
        } else {
            runApiOrders();
        }
    }


    // ════════════════════════════════════════════════════════════════════════
    // PRZEKIEROWANIE DO IDOSELL NA MEGADRON.PL
    // ════════════════════════════════════════════════════════════════════════

    if (url.includes('megadron.pl/pl/products/')) {

        const skuFromUrl = (window.location.pathname.match(/-(\d+)\.html$/) || [])[1];
        if (skuFromUrl) {
            const skuUrl = 'https://client6220.idosell.com/panel/product.php?idt=' + skuFromUrl;

            function waitForElementMegadronLink(selector, callback, timeout = 10000) {
                const el = document.querySelector(selector);
                if (el) { callback(el); return; }
                const observer = new MutationObserver(() => {
                    const el = document.querySelector(selector);
                    if (el) { observer.disconnect(); callback(el); }
                });
                observer.observe(document.body, { childList: true, subtree: true });
                setTimeout(() => observer.disconnect(), timeout);
            }

            waitForElementMegadronLink('.product_name__actions', function (actionsBar) {
                const badge = document.createElement('a');
                badge.href = skuUrl;
                badge.target = '_blank';
                badge.rel = 'noopener noreferrer';
                badge.style.cssText = `
                padding:4px 10px;
                margin-bottom:8px;
                margin-right:8px;
                background:#cce5ff;
                border:1px solid #004085;
                border-radius:4px;
                font-size:14px;
                font-weight:bold;
                display:inline-block;
                width:fit-content;
                cursor:pointer;
                color:#004085;
                text-decoration:none;';
                badge.innerHTML = 'IDOSELL';
                actionsBar.insertAdjacentElement('afterbegin', badge);
                `
            });
        }
    }


    // ════════════════════════════════════════════════════════════════════════
    // LOKALIZACJA NA MAG. Z IDOSELL NA MEGADRON.PL
    // ════════════════════════════════════════════════════════════════════════

    if (url.includes('megadron.pl/pl/products/')) {

        const idtFromUrl2 = (window.location.pathname.match(/-(\d+)\.html$/) || [])[1];
        if (idtFromUrl2) {
            function waitForElementLokalizacja(selector, callback, timeout = 10000) {
                const el = document.querySelector(selector);
                if (el) { callback(el); return; }
                const observer = new MutationObserver(() => {
                    const el = document.querySelector(selector);
                    if (el) { observer.disconnect(); callback(el); }
                });
                observer.observe(document.body, { childList: true, subtree: true });
                setTimeout(() => observer.disconnect(), timeout);
            }

            waitForElementLokalizacja('.product_name__actions', function (actionsBar) {
                const badge = document.createElement('div');
                badge.style.cssText = `
                padding:4px 10px;
                margin-bottom:8px;
                margin-right:20px;
                background:#fff3cd;
                border:1px solid #ffc107;
                border-radius:4px;
                font-size:14px;
                display:inline-flex;
                align-items:center;
                white-space:nowrap;
                width:fit-content;
                `;
                badge.innerHTML = '📦 Lokalizacja: <strong>ładowanie...</strong>';
                actionsBar.insertAdjacentElement('afterbegin', badge);

                GM_xmlhttpRequest({
                    method: 'POST',
                    url: 'https://client6220.idosell.com/panel/ajax/product-location.php',
                    headers: {
                        'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
                        'X-Requested-With': 'XMLHttpRequest',
                        'Referer': `https://client6220.idosell.com/panel/product-edit.php?idt=${idtFromUrl2}`
                    },
                    data: `action=getLocations&product=${idtFromUrl2}`,
                    onload: function (response) {
                        try {
                            const json = JSON.parse(response.responseText);
                            const doc = new DOMParser().parseFromString(json.htmlData, 'text/html');
                            const inputs = doc.querySelectorAll('input[type="text"][name*="location_id_sel[1]"]');
                            let lokalizacja = null;
                            inputs.forEach(input => {
                                if (input.value && input.value.trim() !== '' && !lokalizacja) {
                                    lokalizacja = input.value.trim();
                                }
                            });
                            if (lokalizacja) {
                                badge.innerHTML = `📦: <strong>${lokalizacja}</strong>`;
                                badge.style.background = '#d4edda';
                                badge.style.borderColor = '#28a745';
                            } else {
                                badge.innerHTML = '📦: <strong>brak</strong>';
                                badge.style.background = '#f8f9fa';
                                badge.style.borderColor = '#6c757d';
                            }
                        } catch (e) {
                            badge.innerHTML = '📦: <strong>błąd parsowania</strong>';
                            badge.style.background = '#f8d7da';
                            badge.style.borderColor = '#dc3545';
                        }
                    },
                    onerror: function () {
                        badge.innerHTML = '📦 Lokalizacja: <strong>błąd połączenia</strong>';
                        badge.style.background = '#f8d7da';
                        badge.style.borderColor = '#dc3545';
                    }
                });
            });
        }
    }


    // ════════════════════════════════════════════════════════════════════════
    // STAN DYSPOZYCYJNY M1 NA MEGADRON.PL
    // ════════════════════════════════════════════════════════════════════════

    if (url.includes('megadron.pl/pl/products/')) {

        const idtMgdrn = (window.location.pathname.match(/-(\d+)\.html$/) || [])[1];
        if (idtMgdrn) {
            function waitForElementDysp(selector, callback, timeout = 10000) {
                const el = document.querySelector(selector);
                if (el) { callback(el); return; }
                const observer = new MutationObserver(() => {
                    const el = document.querySelector(selector);
                    if (el) { observer.disconnect(); callback(el); }
                });
                observer.observe(document.body, { childList: true, subtree: true });
                setTimeout(() => observer.disconnect(), timeout);
            }

            waitForElementDysp('.product_name__actions', function (actionsBar) {
                const badge = document.createElement('div');
                badge.style.cssText = `
                padding:4px 10px;
                margin-bottom:8px;
                margin-right:8px;
                background:#fff3cd;
                border:1px solid #ffc107;
                border-radius:4px;
                font-size:14px;
                display:inline-flex;
                align-items:center;
                white-space:nowrap;
                width:fit-content;
                `;
                badge.innerHTML = '🔵 Dyspozycyjny M1: <strong>ładowanie...</strong>';
                actionsBar.insertAdjacentElement('afterbegin', badge);

                GM_xmlhttpRequest({
                    method: 'POST',
                    url: `https://client6220.idosell.com/panel/ajax/product-edit-aceform-tables.php?name=reserved&idt=${idtMgdrn}&filtered=false`,
                    headers: {
                        'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
                        'X-Requested-With': 'XMLHttpRequest',
                        'Referer': `https://client6220.idosell.com/panel/product-edit.php?idt=${idtMgdrn}`
                    },
                    data: `id=stocksReserved&url=/panel/ajax/product-edit-aceform-tables.php%3Fname%3Dreserved%26idt%3D${idtMgdrn}%26filtered%3Dfalse`,
                    onload: function (response) {
                        try {
                            const doc = new DOMParser().parseFromString(response.responseText, 'text/html');
                            const row = doc.querySelector('table tbody tr');
                            if (!row) {
                                badge.innerHTML = '🔵 Dyspozycyjny M1: <strong>brak danych</strong>';
                                return;
                            }
                            const cells = row.querySelectorAll('td');
                            const raw = cells[1] ? cells[1].textContent.trim() : '-';
                            const val = raw === '-' ? 0 : parseInt(raw, 10);
                            const disp = isNaN(val) ? 0 : val;
                            badge.style.background = '#FDF5E6';
                            badge.style.borderColor = '#D2B48C';
                            badge.innerHTML = `🔵 Dyspozycyjny M1: <strong>${disp}</strong>`;
                        } catch (e) {
                            badge.innerHTML = '🔵 Dyspozycyjny M1: <strong>błąd</strong>';
                            badge.style.background = '#f8d7da';
                            badge.style.borderColor = '#dc3545';
                        }
                    },
                    onerror: function () {
                        badge.innerHTML = '🔵 Dyspozycyjny M1: <strong>błąd połączenia</strong>';
                        badge.style.background = '#f8d7da';
                        badge.style.borderColor = '#dc3545';
                    }
                });
            });
        }
    }

})();
