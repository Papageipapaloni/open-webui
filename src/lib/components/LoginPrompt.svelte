<!-- webui/src/lib/components/LoginPrompt.svelte -->
<script lang="ts">
  import { createEventDispatcher, onMount, onDestroy } from "svelte";
  import { browser } from '$app/environment';
  
  const dispatch = createEventDispatcher();

  function handleKey(e: KeyboardEvent) {
    if (e.key === "Escape") {
      dispatch("close");
    }
  }

  onMount(() => {
    window.addEventListener("keydown", handleKey);
    document.body.style.overflow = "hidden"; // Prevent background scrolling
  });
  
  onDestroy(() => {
    window.removeEventListener("keydown", handleKey);
    document.body.style.overflow = "";
  });

  function close() {
    dispatch("close");
  }
</script>

<style>
  .overlay {
    position: fixed;
    inset: 0;
    background-color: rgba(0, 0, 0, 0.7);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
    overflow-y: auto; /* Ermöglicht Scrollen im Overlay */
    padding: 1rem; /* Sicherheitsabstand */
  }
  .modal {
    background: white;
    border-radius: 1rem;
    padding: 2rem;
    max-width: 36rem;
    width: 90%;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
    max-height: 85vh; /* Begrenzt die Höhe */
    overflow-y: auto; /* Macht den Inhalt scrollbar */
    margin: auto; /* Zentriert im flexbox */
  }
  
  /* Verbesserte Scrollbarkeit auf Mobilgeräten */
  @media (max-width: 640px) {
    .modal {
      max-height: 80vh;
      padding: 1.5rem;
      width: 95%;
    }
    
    .overlay {
      align-items: flex-start;
      padding-top: 2rem;
      padding-bottom: 2rem;
    }
    
    /* Kleinere Schriftgröße auf kleinen Bildschirmen */
    .modal h2 {
      font-size: 1.5rem;
    }
    
    .modal h3 {
      font-size: 1.25rem;
    }
    
    .modal p {
      font-size: 1rem;
    }
    
    /* Sticky Footer für bessere UX */
    .modal-footer {
      position: sticky;
      bottom: -1.5rem;
      background-color: white;
      padding-top: 1rem;
      margin-bottom: -1.5rem;
      padding-bottom: 1.5rem;
      border-top: 1px solid #e5e7eb;
    }
    
    /* Dunkel-Modus-Unterstützung für Sticky Footer */
    @media (prefers-color-scheme: dark) {
      .modal-footer {
        background-color: #1a202c;
        border-top-color: #2d3748;
      }
    }
  }
  
  /* Bestehende Styles beibehalten */
  .modal h2 {
    font-size: 1.75rem;
    margin-bottom: 1rem;
    color: #1a202c;
    font-weight: 600;
  }
  .modal h3 {
    font-size: 1.5rem;
    margin-bottom: 0.5rem;
    color: #2d3748;
    font-weight: 500;
  }
  .modal p {
    margin-bottom: 1rem;
    font-size: 1.1rem;
    line-height: 1.6;
    color: #4a5568;
  }
  .modal .highlight {
    font-weight: 600;
    color: #2d3748;
  }
  .modal .section {
    margin-bottom: 1.5rem;
  }
  .modal .disclaimer {
    font-size: 0.9rem;
    color: #718096;
    border-top: 1px solid #e2e8f0;
    padding-top: 1rem;
    margin-top: 1rem;
  }
  .modal button {
    padding: 0.75rem 1.5rem;
    background-color: #2563eb;
    color: white;
    border: none;
    border-radius: 0.5rem;
    cursor: pointer;
    font-size: 1.1rem;
    font-weight: 500;
    transition: background-color 0.2s;
  }
  .modal button:hover {
    background-color: #1d4ed8;
  }
  .modal-footer {
    display: flex;
    justify-content: flex-end;
    margin-top: 1.5rem;
  }
  .modal-header {
    display: flex;
    align-items: center;
    margin-bottom: 1.5rem;
  }
  .modal-icon {
    margin-right: 1rem;
    color: #2563eb;
    font-size: 1.5rem;
  }
  .feature-list {
    list-style-type: none;
    padding-left: 1.5rem;
    margin-bottom: 1rem;
  }
  .feature-list li {
    position: relative;
    padding-left: 1rem;
    margin-bottom: 0.5rem;
  }
  .feature-list li:before {
    content: "•";
    position: absolute;
    left: 0;
    color: #2563eb;
  }
  .numbered-list {
    counter-reset: list-counter;
    padding-left: 1.5rem;
  }
  .numbered-list li {
    counter-increment: list-counter;
    margin-bottom: 0.5rem;
    position: relative;
  }
  .numbered-list li:before {
    content: counter(list-counter) ".";
    position: absolute;
    left: -1.5rem;
    color: #2563eb;
  }
  .note {
    font-size: 0.9rem;
    color: #b23218;
    margin-top: 0.5rem;
  }
  .examples {
    font-style: italic;
    color: #2d3748;
    margin-top: 0.5rem;
  }
  @media (prefers-color-scheme: dark) {
    .modal {
      background: #1a202c;
    }
    .modal h2 {
      color: #f7fafc;
    }
    .modal h3 {
      color: #edf2f4;
    }
    .modal p {
      color: #cbd5e0;
    }
    .modal .highlight {
      color: #edf2f4;
    }
    .modal .section {
      border-top: 1px solid #2d3748;
      padding-top: 1rem;
      margin-top: 1rem;
    }
    .modal .disclaimer {
      color: #a0aec0;
      border-top: 1px solid #2d3748;
      padding-top: 1rem;
      margin-top: 1rem;
    }
    .modal button {
      background-color: #4f46e5;
    }
    .modal button:hover {
      background-color: #4338ca;
    }
    
    .blue-highlight {
      color: #3b82f6; /* Etwas helleres Blau für besseren Kontrast im Dark Mode */
    }
  }
  
  .blue-highlight {
    color: #456ec8;
    font-weight: 600;
  }
  
  .important-note {
    background-color: #fef2f2;
    border-left: 4px solid #b91c1c;
    padding: 0.8rem 1rem;
    margin: 1rem 0 1rem 0;
    color: #991b1b;
    font-weight: 500;
  }

  @media (prefers-color-scheme: dark) {
    .important-note {
      background-color: rgba(185, 28, 28, 0.1);
      border-left-color: #b91c1c;
      color: #fca5a5;
    }
  }
</style>

<div class="overlay" role="dialog" aria-modal="true">
  <div class="modal" tabindex="0">
    <h2>Kurzanleitung</h2>
    
    <div class="section">
      <h3>Anmeldung</h3>
      <p>
        Um fortzufahren, müssen Sie sich bitte zunächst registrieren. Bitte geben Sie <strong>KEINE</strong> echte E-Mail-Adresse und <strong>NICHT</strong> Ihren echten Namen an! Es funktionieren erfundene E-Mail-Adressen wie z.&nbsp;B. <strong>anonym@anonym.de</strong>.
      </p>
      <p> 
        <span class="blue-highlight">Die gesamte Datenverarbeitung erfolgt ausschließlich lokal auf einem privaten Server – ohne externe Dienste oder Drittanbieter. Alle Ihre Daten sind sicher und anonym! Die Teilnahme erfolgt vollständig anonym!</span>
      </p>
    </div>
    
    <div class="section">
      <h3>Ablauf der Studie</h3>
      <ol class="numbered-list">
        <li>
          Nach der Registrierung/Anmeldung sehen Sie ein Chatfenster, in dem Sie mit der ersten KI <span class="highlight">"Bot A"</span> schreiben können.
        </li>

        <div class="important-note">
          <strong>Wichtig:</strong> Beide KIs können <strong>nur</strong> auf <strong>Englisch</strong> kommunizieren. Schreiben Sie Ihre Nachrichten daher bitte ausschließlich auf Englisch.
        </div>
        
        <li>
          Schreiben Sie nun bitte mit <span class="highlight">"Bot A"</span> über ein psychotherapeutisches Thema Ihrer Wahl.
          <div class="examples">
            Zum Beispiel: "I feel sad" oder "I'm anxious about my exams".
          </div>
        </li>
        
        <li>
          Bitte schreiben Sie <span class="highlight">30 Nachrichten</span>. Sobald Sie diese Anzahl erreicht haben, werden Sie automatisch von <span class="highlight">"Bot A"</span> zu <span class="highlight">"Bot B"</span> weitergeleitet.
        </li>
        
        <li>
          Schreiben Sie auch mit <span class="highlight">"Bot B"</span> über das <strong>gleiche</strong> psychotherapeutische Thema wie zuvor mit <span class="highlight">"Bot A"</span>. Zur besseren Vergleichbarkeit wird Ihre erste Nachricht an <span class="highlight">"Bot B"</span> automatisch aus der ersten Nachricht, die Sie an <span class="highlight">"Bot A"</span> geschrieben haben, kopiert.
        </li>
        
        <li>
          Nach <span class="highlight">30 Nachrichten</span> mit Bot B werden Sie automatisch zu einer <span class="highlight">Umfrage</span> weitergeleitet. Bitte füllen Sie diese <span class="highlight">vollständig</span> bis zum Ende aus.
        </li>
      </ol>
      
      <div class="blue-highlight">
        <strong>Hinweis zur Ladedauer:</strong> Bitte haben Sie etwas Geduld, während die KI eine Antwort generiert. Bei der Erzeugung der Antworten kann es gelegentlich zu kurzen Wartezeiten kommen.
      </div>

      <div class="important-note">
        <strong>Wichtig:</strong> Diese KI ist <strong>kein</strong> medizinischer oder psychotherapeutischer Service und kann falsche oder schädliche Ratschläge geben. In <strong>Notfällen</strong> wenden Sie sich bitte an Fachleute unter: <strong>116117, 0800&nbsp;1110111 (Telefonseelsorge)</strong> oder an <strong>112</strong>. Nutzung auf eigene Gefahr!
      </div>
    
    <div class="modal-footer">
      <button on:click={close}>Verstanden</button>
    </div>
  </div>
</div>
</div>
