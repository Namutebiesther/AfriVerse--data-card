/**
 * @license
 *
 * Copyright 2022 Google LLC. All Rights Reserved.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

setCollapsibleMenu();
setHeaderParallax();

setCollapsibleTableOfContents();
setUserGuideTemplateHover();
setLibraryFilters();
setCardBuilder();

/* Set toggle for collapsible main menu (in small viewports) */
function setCollapsibleMenu() {
    document.getElementById('menu-expand-button').addEventListener(
        'click',
        (e) => toggleClass(e.currentTarget.parentElement.parentElement)
    );
}

/* Set accordion toggle for article table of contents */
function setCollapsibleTableOfContents() {
    for (element of document.getElementsByClassName('chapter-title')) {
        element.addEventListener('click', (e) =>
            toggleClass(e.currentTarget.parentElement));
    }
}

/* Toggles given class name */
function toggleClass(element, className = 'open') {
    element.classList.toggle(className);
}

/* Set parallax for header */
function setHeaderParallax() {
    headerContent = document.getElementById('header-parallax');
    dotGrid = document.getElementById('dot-grid');
    window.addEventListener('scroll', () => {
        if (headerContent) {
            headerContent.style.top = -1 * window.scrollY + 'px';
        }
        if (dotGrid) {
            dotGrid.style.top = -.25 * window.scrollY + 'px';
        }
    });
}

/* Set hover cards for User Guide template */
function setUserGuideTemplateHover() {
    template = document.getElementById('guide-template');
    if (template) {
        template.classList.add('active');
        for (card of template.getElementsByClassName('hover card')) {
            card.classList.add('hidden');
        }
        for (button of template.getElementsByClassName('hover-button')) {
            button.addEventListener('click', (e) => {
                current = e.currentTarget;
                for (let i = 1; i <= 6; i++) {
                    if (`hover-${i}` != current.id) {
                        document.getElementById(`hover-${i}`).classList.remove('active');
                        document.getElementById(`card-hover-${i}`).classList.add('hidden');
                    }
                }
                current.classList.toggle('active');
                document.getElementById('card-' + current.id).classList.toggle('hidden');
            });
        }
    }
}

/* Set filters and views for card library */
function setLibraryFilters() {
    const library = document.getElementById('card-library');
    const filters = document.getElementById('card-library-filters');
    
    const instructions = document.createElement('article');
    instructions.className = 'card-instructions';
    instructions.innerHTML = '<p>No cards found for the current filter settings.</p>';
    
    // Set accordion toggle for fieldsets
    if (filters) {
        const fieldsets = filters.getElementsByTagName('legend');
        for (element of fieldsets) {
            element.addEventListener('click', (e) =>
                toggleClass(e.currentTarget.parentElement));
        }
    }
    
    // Set view toggle for card library
    if (library) {
        const setViewToggle = (viewType) => {
            const view = document.getElementById(`view-${viewType}`);
            if (view) {
                view.addEventListener('change', () => {
                    library.classList.toggle('list', viewType == 'list');
                    library.classList.toggle('grid', viewType == 'grid');
                });
            }
        }
        
        setViewToggle('grid');
        setViewToggle('list');
    }
    
    // Set filters for card library
    if (filters && library) {
        const unusedFields = [];
    
        for (input of filters.getElementsByTagName('input')) {
            if (input.type == 'checkbox') {
                if (library.getElementsByClassName(input.id).length == 0) {
                    unusedFields.push(input.parentElement);
                } else {
                    input.addEventListener('change', (e) => {
                        const box = e.currentTarget;
                        filterCardsByClassName(library, box.id, box.checked, instructions)
                    });
                    const numCards = library.getElementsByClassName(input.id).length;
                    input.nextElementSibling.innerHTML += ` (${numCards})`;
                }
            }
        }
        unusedFields.forEach(e => e.remove());
    
        const setButton = (id, showCards) => {
            const button = document.getElementById(id);
            if (button) {
                button.addEventListener('click', () => {
                    for (input of filters.getElementsByTagName('input')) {
                        if (input.type == 'checkbox') {
                            filterCardsByClassName(library, input.id, showCards, instructions);
                        }
                    }
                });
            }
        }
        
        setButton('button-check-all', true);
        setButton('button-check-none', false);
    }
}

function filterCardsByClassName(cards, className, includeCard, instructions) {
    // Set checkbox to value of includeCard
    const checkbox = document.getElementById(className);
    if (checkbox) {
        checkbox.checked = includeCard;
        checkbox.disabled = false;
    }

    // Set card visibility and save class names
    const classNames = new Set();
    for (card of cards.getElementsByClassName(className)) {
        card.classList.toggle('hidden', !includeCard);
        for (c of card.classList) {
            classNames.add(c);
        }
    }

    // Display/hide "no cards found" message
    if (includeCard && instructions.parentElement) {
        instructions.remove();
    } else if (cards.getElementsByClassName('card').length ==
            cards.getElementsByClassName('card hidden').length) {
        cards.append(instructions);
    }
}

/* Set up interactions for Card Builder */
function setCardBuilder() {
    addCopyListener('copy-markdown', 'markdown-textarea', true);
    addCopyListener('copy-html', 'datacard-preview');

    const renderButton = document.getElementById('markdown-to-card-button');
    const renderInput = document.getElementById('markdown-textarea');

    if (renderButton && renderInput) {
        renderButton.addEventListener('click', (e) => renderCard(renderInput, renderButton));
        renderInput.addEventListener('input', () => { renderButton.disabled = false; });
    }
}

function renderCard(renderInput, renderButton) {
    renderButton.disabled = true;
    const preview = document.getElementById('datacard-preview');
    const newPreview = CardBuilder.markdownToHTMLCard(renderInput.value);
    newPreview.id = 'datacard-preview';
    preview.replaceWith(newPreview);
}

function addCopyListener(copyButtonId, copyContentId, isTextArea = false) {
    const copyButton = document.getElementById(copyButtonId);
    const copyContent = document.getElementById(copyContentId);
    if (copyButton && copyContent) {
        copyButton.addEventListener('click', (e) => {
            if (isTextArea) {
                navigator.clipboard.writeText(copyContent.value);
            } else {
                const content = document.getElementById(copyContentId);
                navigator.clipboard.writeText(content.outerHTML);
            }
        });
        copyButton.disabled = false;
    }
}
