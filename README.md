import { useState, useEffect, useRef } from "react";

/* ─── THEME TOKENS ──────────────────────────────────────────────────────── */
const DARK = {
  "--bg": "#0D0E0F", "--surface": "#141618", "--card": "#1C1F22",
  "--border": "#2A2E33", "--border-dim": "#1E2226",
  "--lime": "#C8F135", "--lime-dim": "#A8D020", "--lime-glow": "rgba(200,241,53,0.12)",
  "--blue": "#4B7BF5", "--blue-dim": "#3460D8", "--blue-glow": "rgba(75,123,245,0.12)",
  "--fg": "#F2F4F5", "--fg2": "#7A8490", "--fg3": "#3E454D",
  "--hero-grid-opacity": "0.45",
};
const LIGHT = {
  "--bg": "#F7F8FA", "--surface": "#FFFFFF", "--card": "#EEF0F4",
  "--border": "#DDE1E8", "--border-dim": "#E8EBF0",
  "--lime": "#7AAD00", "--lime-dim": "#5E8800", "--lime-glow": "rgba(122,173,0,0.10)",
  "--blue": "#2B5CE6", "--blue-dim": "#1A46C8", "--blue-glow": "rgba(43,92,230,0.10)",
  "--fg": "#0D0E0F", "--fg2": "#5A6270", "--fg3": "#9AA3AF",
  "--hero-grid-opacity": "0.25",
};

/* ─── TRANSLATIONS ──────────────────────────────────────────────────────── */
const LANGS = [
  { code: "en", label: "English",    flag: "🇬🇧" },
  { code: "es", label: "Español",    flag: "🇪🇸" },
  { code: "fr", label: "Français",   flag: "🇫🇷" },
  { code: "de", label: "Deutsch",    flag: "🇩🇪" },
  { code: "nl", label: "Nederlands", flag: "🇳🇱" },
  { code: "sv", label: "Svenska",    flag: "🇸🇪" },
  { code: "da", label: "Dansk",      flag: "🇩🇰" },
  { code: "no", label: "Norsk",      flag: "🇳🇴" },
  { code: "fi", label: "Suomi",      flag: "🇫🇮" },
];

const T = {
  en: {
    navPersons: "For Individuals", navCompanies: "For Companies",
    navDestinations: "Destinations", navPricing: "Pricing",
    navLogin: "Log in", navCta: "Get started →",
    heroBadge: "Digital Nomad Visa · Spain",
    heroH1a: "Your infrastructure to", heroH1b: "live", heroH1c: "and", heroH1d: "work",
    heroH1e: "in Spain",
    heroSub: "From your first visa to full integration. YouNomad connects visas, landing services and community for digital nomads, freelancers and international students.",
    heroCta1: "Eligibility check →", heroCta2: "See how it works",
    heroPill1: "No endless calls", heroPill2: "No bureaucratic drama", heroPill3: "No lost paperwork",
    trustLabel: "We work with",
    srvLabel: "· What we do ·",
    srvH2a: "Not just another visa agency,", srvH2b: "the backbone of your", srvH2c: "global life",
    srvP: "Behind YouNomad are legal teams, mobility specialists and service providers who accompany thousands of international relocations every year. One place to manage everything.",
    srv1Title: "Visas & Regulation", srv1Desc: "We compare official programmes for nomads, freelancers, students and remote teams. Local legal support and real-time tracking throughout the entire process.",
    srv2Title: "Landing & Daily Life", srv2Desc: "Bank account, verified housing, health insurance, local SIM, coworking and registration. Everything essential ready from day one.",
    srv3Title: "Growth & Community", srv3Desc: "Events, exclusive benefits, professional resources and real connections so your global life is a long-term project, not just a series of trips.",
    srvMore: "Learn more →",
    howLabel: "· How It Works ·",
    howH2a: "A", howH2b: "continuous", howH2c: "journey",
    howP: "The same structure guides you from eligibility all the way to full integration in Spain.",
    step1Title: "Tell us who you are", step1Desc: "Share your profile and situation. Eligibility check in minutes.",
    step2Title: "Step-by-step guidance", step2Desc: "Documents, timelines and specialised legal support at every stage.",
    step3Title: "We prepare your arrival", step3Desc: "Housing, bank account and SIM ready before you land in Spain.",
    step4Title: "With you in your new life", step4Desc: "Ongoing support, active community and professional resources from day one.",
    destLabel: "· Destinations ·",
    destH2a: "Explore where you want to", destH2b: "live", destH2c: "and", destH2d: "work",
    destP: "The most popular cities chosen by the YouNomad community in Spain.",
    destMeta: ["Capital · Digital Nomad", "Tech · Mediterranean", "Culture · Exceptional Climate", "Beach · Startup Hub"],
    destTags: ["Most popular", "Trending", "Growing", "Top pick"],
    stat1Label: "Relocations completed", stat2Label: "Visa approval rate", stat3Label: "Main destinations in Spain",
    testiLabel: "· Testimonials ·",
    testiH2a: "What people say", testiH2b: "about us",
    testiQuotes: [
      "The visa process seemed impossible until I found YouNomad. In less than a month I had everything ready to move to Madrid. Incredible team.",
      "I had spent months trying to understand the requirements. YouNomad simplified everything and the team was available at every step. No stress, no surprises.",
      "From the bank account to the flat in Barcelona, everything was coordinated. I felt supported from day one. Absolutely recommend it.",
    ],
    testiRoles: ["UX Designer · Mexico", "Developer · Ireland", "Consultant · Japan"],
    priceLabel: "· Pricing ·",
    priceH2a: "Choose how you want", priceH2b: "YouNomad to support you",
    priceP: "No hidden costs. Official fees and legal representation always itemised.",
    planNames: ["Visa Core", "Academic + Visa", "Full Relocation"],
    planBadge: "Most chosen",
    planPeriod: "one-time fee",
    planFeatures: [
      ["Eligibility quiz", "Visa route comparison", "Strategic consultation", "Full application preparation", "Document checklist & review"],
      ["Everything in Visa Core", "University selection", "Application management", "Acceptance letter coordination", "TIE accompaniment on arrival"],
      ["Everything above", "Verified housing support", "US → Spain management", "TIE accompaniment", "Priority support throughout"],
    ],
    planCta: "Get started",
    ctaLabel: "· Start Today ·",
    ctaH2a: "Your global life starts", ctaH2b: "with one", ctaH2c: "decision",
    ctaP: "Share your profile. We will guide you through every step and prepare your landing so you arrive ready to live, not just to do paperwork.",
    ctaCta1: "Free eligibility check →", ctaCta2: "Talk to the team",
    footerDesc: "We simplify the digital visa process for nomads in Spain so you can focus on planning your life, not your paperwork.",
    footerCol1: "For Individuals", footerCol2: "For Companies", footerCol3: "YouNomad",
    footerLinks1: ["YouNomad for you", "How we do it", "Spain 360°", "Get started"],
    footerLinks2: ["For your talent", "How we work", "Spain 360°", "Design a plan"],
    footerLinks3: ["About Us", "Eligibility check", "Pricing", "Contact"],
    footerCopy: "© 2026 YouNomad. All rights reserved.",
    footerTagline: "Made with ♥ for global nomads",
  },
  es: {
    navPersons: "Para Personas", navCompanies: "Para Empresas",
    navDestinations: "Destinos", navPricing: "Precios",
    navLogin: "Iniciar sesión", navCta: "Empezar →",
    heroBadge: "Visado Nómada Digital · España",
    heroH1a: "Tu infraestructura para", heroH1b: "vivir", heroH1c: "y", heroH1d: "trabajar",
    heroH1e: "en España",
    heroSub: "Desde el primer visado hasta tu integración total. YouNomad conecta visados, servicios de aterrizaje y comunidad para nómadas digitales, freelancers y estudiantes internacionales.",
    heroCta1: "Chequeo de elegibilidad →", heroCta2: "Ver cómo funciona",
    heroPill1: "Sin llamadas interminables", heroPill2: "Sin drama burocrático", heroPill3: "Sin papeles perdidos",
    trustLabel: "Trabajamos con",
    srvLabel: "· Qué hacemos ·",
    srvH2a: "No otra gestoría,", srvH2b: "la columna vertebral de tu", srvH2c: "vida global",
    srvP: "Detrás de YouNomad hay equipos legales, especialistas en movilidad y proveedores que acompañan miles de traslados internacionales cada año. Un solo lugar para gestionar todo.",
    srv1Title: "Visados y Regulación", srv1Desc: "Comparamos programas oficiales para nómadas, freelancers, estudiantes y equipos remotos. Apoyo legal local y seguimiento en tiempo real.",
    srv2Title: "Aterrizaje y Vida Diaria", srv2Desc: "Cuenta bancaria, vivienda verificada, seguro médico, número local, coworking y empadronamiento. Todo lo esencial listo desde el primer día.",
    srv3Title: "Crecimiento y Comunidad", srv3Desc: "Eventos, beneficios exclusivos, recursos profesionales y conexiones reales para que tu vida global sea un proyecto a largo plazo.",
    srvMore: "Saber más →",
    howLabel: "· Cómo Funciona ·",
    howH2a: "Un recorrido", howH2b: "continuo", howH2c: "",
    howP: "La misma estructura te guía desde la elegibilidad hasta tu integración plena en España.",
    step1Title: "Cuéntanos quién eres", step1Desc: "Comparte tu perfil y situación. Chequeo de elegibilidad en minutos.",
    step2Title: "Guía paso a paso", step2Desc: "Documentación, plazos y apoyo legal especializado en cada etapa.",
    step3Title: "Preparamos tu llegada", step3Desc: "Vivienda, cuenta bancaria y SIM listos antes de aterrizar en España.",
    step4Title: "Contigo en tu nueva vida", step4Desc: "Soporte continuo, comunidad activa y recursos profesionales desde el primer día.",
    destLabel: "· Destinos ·",
    destH2a: "Explora dónde quieres", destH2b: "vivir", destH2c: "y", destH2d: "trabajar",
    destP: "Las ciudades más elegidas por la comunidad YouNomad en España.",
    destMeta: ["Capital · Nómada Digital", "Tech · Mediterráneo", "Cultura · Clima excepcional", "Playa · Startup Hub"],
    destTags: ["Más popular", "Trending", "Crecimiento", "Top pick"],
    stat1Label: "Traslados completados", stat2Label: "Tasa de aprobación de visados", stat3Label: "Destinos principales en España",
    testiLabel: "· Testimonios ·",
    testiH2a: "Lo que dicen", testiH2b: "de nosotros",
    testiQuotes: [
      "El proceso de visado me parecía imposible hasta que llegué a YouNomad. En menos de un mes tenía todo listo para mudarme a Madrid. Equipo increíble.",
      "Llevaba meses intentando entender los requisitos. YouNomad simplificó todo y el equipo estuvo disponible en cada paso. Sin estrés, sin sorpresas.",
      "Desde la cuenta bancaria hasta el piso en Barcelona, todo coordinado. Me sentí acompañada desde el primer día. Absolutamente recomendable.",
    ],
    testiRoles: ["Diseñadora UX · México", "Desarrollador · Irlanda", "Consultora · Japón"],
    priceLabel: "· Precios ·",
    priceH2a: "Elige cómo quieres que", priceH2b: "YouNomad te acompañe",
    priceP: "Sin costes ocultos. Las tasas oficiales y representación legal siempre desglosadas.",
    planNames: ["Visa Core", "Académico + Visado", "Reubicación Completa"],
    planBadge: "Más elegido",
    planPeriod: "tarifa única",
    planFeatures: [
      ["Quiz de elegibilidad", "Comparación de rutas de visado", "Asesoría estratégica", "Preparación completa del expediente", "Checklist y revisión de documentos"],
      ["Todo en Visa Core", "Selección de universidad", "Gestión de solicitudes", "Coordinación carta de aceptación", "Acompañamiento TIE a la llegada"],
      ["Todo lo anterior", "Soporte en vivienda verificada", "Gestión EE. UU. → España", "Acompañamiento TIE", "Soporte prioritario todo el proceso"],
    ],
    planCta: "Empezar ahora",
    ctaLabel: "· Empieza Hoy ·",
    ctaH2a: "Tu vida global empieza", ctaH2b: "con una", ctaH2c: "decisión",
    ctaP: "Comparte tu perfil. Te guiaremos en cada paso y prepararemos tu aterrizaje para que llegues listo para vivir, no solo para hacer trámites.",
    ctaCta1: "Chequeo de elegibilidad gratuito →", ctaCta2: "Hablar con el equipo",
    footerDesc: "Simplificamos el proceso de visa digital para nómadas en España para que te centres en planificar tu vida, no tus papeles.",
    footerCol1: "Para Individuales", footerCol2: "Para Empresas", footerCol3: "YouNomad",
    footerLinks1: ["YouNomad para ti", "Cómo lo hacemos", "España 360°", "Empezar"],
    footerLinks2: ["Para tu talento", "Cómo trabajamos", "España 360°", "Diseñar un plan"],
    footerLinks3: ["Acerca de Nosotros", "Chequeo de elegibilidad", "Precios", "Contacto"],
    footerCopy: "© 2026 YouNomad. Todos los derechos reservados.",
    footerTagline: "Hecho con ♥ para nómadas globales",
  },
  fr: {
    navPersons: "Pour les particuliers", navCompanies: "Pour les entreprises",
    navDestinations: "Destinations", navPricing: "Tarifs",
    navLogin: "Se connecter", navCta: "Commencer →",
    heroBadge: "Visa Nomade Digital · Espagne",
    heroH1a: "Votre infrastructure pour", heroH1b: "vivre", heroH1c: "et", heroH1d: "travailler",
    heroH1e: "en Espagne",
    heroSub: "Du premier visa jusqu'à votre intégration totale. YouNomad connecte visas, services d'installation et communauté pour les nomades numériques, freelances et étudiants internationaux.",
    heroCta1: "Vérification d'éligibilité →", heroCta2: "Voir comment ça marche",
    heroPill1: "Sans appels interminables", heroPill2: "Sans galères administratives", heroPill3: "Sans documents perdus",
    trustLabel: "Nous travaillons avec",
    srvLabel: "· Ce que nous faisons ·",
    srvH2a: "Pas encore une agence de visas,", srvH2b: "l'épine dorsale de votre", srvH2c: "vie globale",
    srvP: "Derrière YouNomad se trouvent des équipes juridiques, des spécialistes de la mobilité et des prestataires qui accompagnent des milliers de déménagements internationaux chaque année.",
    srv1Title: "Visas & Réglementation", srv1Desc: "Nous comparons les programmes officiels pour nomades, freelances, étudiants et équipes à distance. Soutien juridique local et suivi en temps réel.",
    srv2Title: "Installation & Vie quotidienne", srv2Desc: "Compte bancaire, logement vérifié, assurance santé, SIM local, coworking et enregistrement. Tout l'essentiel prêt dès le premier jour.",
    srv3Title: "Croissance & Communauté", srv3Desc: "Événements, avantages exclusifs, ressources professionnelles et vraies connexions pour que votre vie globale soit un projet à long terme.",
    srvMore: "En savoir plus →",
    howLabel: "· Comment ça marche ·",
    howH2a: "Un parcours", howH2b: "continu", howH2c: "",
    howP: "La même structure vous guide de l'éligibilité jusqu'à votre intégration complète en Espagne.",
    step1Title: "Parlez-nous de vous", step1Desc: "Partagez votre profil et situation. Vérification d'éligibilité en quelques minutes.",
    step2Title: "Guide étape par étape", step2Desc: "Documents, délais et soutien juridique spécialisé à chaque étape.",
    step3Title: "Nous préparons votre arrivée", step3Desc: "Logement, compte bancaire et SIM prêts avant votre atterrissage en Espagne.",
    step4Title: "À vos côtés dans votre nouvelle vie", step4Desc: "Soutien continu, communauté active et ressources professionnelles dès le premier jour.",
    destLabel: "· Destinations ·",
    destH2a: "Explorez où vous voulez", destH2b: "vivre", destH2c: "et", destH2d: "travailler",
    destP: "Les villes les plus choisies par la communauté YouNomad en Espagne.",
    destMeta: ["Capitale · Nomade Digital", "Tech · Méditerranée", "Culture · Climat exceptionnel", "Plage · Hub Startup"],
    destTags: ["Le plus populaire", "Tendance", "En croissance", "Top choix"],
    stat1Label: "Déménagements réalisés", stat2Label: "Taux d'approbation des visas", stat3Label: "Destinations principales en Espagne",
    testiLabel: "· Témoignages ·",
    testiH2a: "Ce que disent", testiH2b: "nos clients",
    testiQuotes: [
      "Le processus de visa me semblait impossible jusqu'à ce que je trouve YouNomad. En moins d'un mois, tout était prêt pour m'installer à Madrid. Équipe incroyable.",
      "Je passais des mois à essayer de comprendre les exigences. YouNomad a tout simplifié et l'équipe était disponible à chaque étape. Sans stress, sans surprises.",
      "Du compte bancaire à l'appartement à Barcelone, tout était coordonné. Je me suis sentie accompagnée dès le premier jour. Je recommande vivement.",
    ],
    testiRoles: ["Designer UX · Mexique", "Développeur · Irlande", "Consultante · Japon"],
    priceLabel: "· Tarifs ·",
    priceH2a: "Choisissez comment vous voulez que", priceH2b: "YouNomad vous accompagne",
    priceP: "Pas de frais cachés. Les frais officiels et la représentation juridique toujours détaillés.",
    planNames: ["Visa Core", "Académique + Visa", "Relocation Complète"],
    planBadge: "Le plus choisi",
    planPeriod: "frais uniques",
    planFeatures: [
      ["Quiz d'éligibilité", "Comparaison des parcours de visa", "Consultation stratégique", "Préparation complète du dossier", "Check-list et révision des documents"],
      ["Tout dans Visa Core", "Sélection d'université", "Gestion des candidatures", "Coordination lettre d'acceptation", "Accompagnement TIE à l'arrivée"],
      ["Tout ce qui précède", "Soutien logement vérifié", "Gestion États-Unis → Espagne", "Accompagnement TIE", "Support prioritaire tout au long"],
    ],
    planCta: "Commencer maintenant",
    ctaLabel: "· Commencez Aujourd'hui ·",
    ctaH2a: "Votre vie globale commence", ctaH2b: "avec une", ctaH2c: "décision",
    ctaP: "Partagez votre profil. Nous vous guiderons à chaque étape et préparerons votre arrivée pour que vous arriviez prêt à vivre, pas seulement à faire des démarches.",
    ctaCta1: "Vérification d'éligibilité gratuite →", ctaCta2: "Parler à l'équipe",
    footerDesc: "Nous simplifions le processus de visa numérique pour les nomades en Espagne afin que vous puissiez vous concentrer sur votre vie, pas vos papiers.",
    footerCol1: "Pour les particuliers", footerCol2: "Pour les entreprises", footerCol3: "YouNomad",
    footerLinks1: ["YouNomad pour vous", "Comment nous faisons", "Espagne 360°", "Commencer"],
    footerLinks2: ["Pour vos talents", "Comment nous travaillons", "Espagne 360°", "Concevoir un plan"],
    footerLinks3: ["À propos de nous", "Vérification d'éligibilité", "Tarifs", "Contact"],
    footerCopy: "© 2026 YouNomad. Tous droits réservés.",
    footerTagline: "Fait avec ♥ pour les nomades du monde",
  },
  de: {
    navPersons: "Für Einzelpersonen", navCompanies: "Für Unternehmen",
    navDestinations: "Reiseziele", navPricing: "Preise",
    navLogin: "Anmelden", navCta: "Loslegen →",
    heroBadge: "Digitales Nomaden-Visum · Spanien",
    heroH1a: "Deine Infrastruktur zum", heroH1b: "Leben", heroH1c: "und", heroH1d: "Arbeiten",
    heroH1e: "in Spanien",
    heroSub: "Vom ersten Visum bis zur vollständigen Integration. YouNomad verbindet Visa, Einreisedienste und Community für digitale Nomaden, Freelancer und internationale Studierende.",
    heroCta1: "Eignungscheck →", heroCta2: "Wie es funktioniert",
    heroPill1: "Keine endlosen Anrufe", heroPill2: "Kein bürokratischer Stress", heroPill3: "Keine verlorenen Dokumente",
    trustLabel: "Wir arbeiten mit",
    srvLabel: "· Was wir tun ·",
    srvH2a: "Nicht noch eine Visa-Agentur,", srvH2b: "das Rückgrat deines", srvH2c: "globalen Lebens",
    srvP: "Hinter YouNomad stehen Rechtsteams, Mobilitätsspezialisten und Dienstleister, die jedes Jahr tausende internationale Umzüge begleiten. Ein einziger Ort für alles.",
    srv1Title: "Visa & Regulierung", srv1Desc: "Wir vergleichen offizielle Programme für Nomaden, Freelancer, Studierende und Remote-Teams. Lokale Rechtsunterstützung und Echtzeit-Tracking.",
    srv2Title: "Einreise & Alltag", srv2Desc: "Bankkonto, verifiziertes Wohnen, Krankenversicherung, lokale SIM, Coworking und Anmeldung. Alles Wesentliche ab Tag eins bereit.",
    srv3Title: "Wachstum & Community", srv3Desc: "Veranstaltungen, exklusive Vorteile, berufliche Ressourcen und echte Verbindungen, damit dein globales Leben ein langfristiges Projekt wird.",
    srvMore: "Mehr erfahren →",
    howLabel: "· Wie es funktioniert ·",
    howH2a: "Eine", howH2b: "kontinuierliche", howH2c: "Reise",
    howP: "Dieselbe Struktur begleitet dich von der Eignung bis zur vollständigen Integration in Spanien.",
    step1Title: "Erzähl uns von dir", step1Desc: "Teile dein Profil und deine Situation. Eignungscheck in wenigen Minuten.",
    step2Title: "Schritt-für-Schritt-Begleitung", step2Desc: "Dokumente, Fristen und spezialisierte Rechtsunterstützung in jeder Phase.",
    step3Title: "Wir bereiten deine Ankunft vor", step3Desc: "Wohnung, Bankkonto und SIM bereit, bevor du in Spanien landest.",
    step4Title: "An deiner Seite im neuen Leben", step4Desc: "Fortlaufende Unterstützung, aktive Community und berufliche Ressourcen von Tag eins an.",
    destLabel: "· Reiseziele ·",
    destH2a: "Erkunde, wo du", destH2b: "leben", destH2c: "und", destH2d: "arbeiten möchtest",
    destP: "Die beliebtesten Städte der YouNomad-Community in Spanien.",
    destMeta: ["Hauptstadt · Digitaler Nomade", "Tech · Mittelmeer", "Kultur · Außergewöhnliches Klima", "Strand · Startup-Hub"],
    destTags: ["Am beliebtesten", "Im Trend", "Wachsend", "Top-Wahl"],
    stat1Label: "Abgeschlossene Umzüge", stat2Label: "Visum-Genehmigungsrate", stat3Label: "Hauptreiseziele in Spanien",
    testiLabel: "· Erfahrungsberichte ·",
    testiH2a: "Was unsere Kunden", testiH2b: "sagen",
    testiQuotes: [
      "Der Visaprozess schien unmöglich, bis ich YouNomad fand. In weniger als einem Monat war alles bereit für meinen Umzug nach Madrid. Unglaubliches Team.",
      "Ich hatte monatelang versucht, die Anforderungen zu verstehen. YouNomad hat alles vereinfacht und das Team war in jedem Schritt dabei. Kein Stress, keine Überraschungen.",
      "Vom Bankkonto bis zur Wohnung in Barcelona – alles koordiniert. Ich fühlte mich von Tag eins an begleitet. Absolute Empfehlung.",
    ],
    testiRoles: ["UX-Designerin · Mexiko", "Entwickler · Irland", "Beraterin · Japan"],
    priceLabel: "· Preise ·",
    priceH2a: "Wähle, wie du möchtest, dass", priceH2b: "YouNomad dich begleitet",
    priceP: "Keine versteckten Kosten. Offizielle Gebühren und rechtliche Vertretung immer aufgeschlüsselt.",
    planNames: ["Visa Core", "Akademisch + Visum", "Kompletter Umzug"],
    planBadge: "Am meisten gewählt",
    planPeriod: "einmalige Gebühr",
    planFeatures: [
      ["Eignungsquiz", "Visa-Routen-Vergleich", "Strategische Beratung", "Vollständige Antragsvorbereitung", "Dokumenten-Checkliste & Überprüfung"],
      ["Alles in Visa Core", "Universitätsauswahl", "Bewerbungsmanagement", "Koordination Zulassungsschreiben", "TIE-Begleitung bei Ankunft"],
      ["Alles oben genannte", "Verifizierter Wohnungsservice", "USA → Spanien-Management", "TIE-Begleitung", "Vorrangiger Support durchgehend"],
    ],
    planCta: "Jetzt starten",
    ctaLabel: "· Heute starten ·",
    ctaH2a: "Dein globales Leben beginnt", ctaH2b: "mit einer", ctaH2c: "Entscheidung",
    ctaP: "Teile dein Profil. Wir begleiten dich durch jeden Schritt und bereiten deine Einreise vor, damit du bereit bist zu leben, nicht nur Formalitäten zu erledigen.",
    ctaCta1: "Kostenloser Eignungscheck →", ctaCta2: "Mit dem Team sprechen",
    footerDesc: "Wir vereinfachen den digitalen Visaprozess für Nomaden in Spanien, damit du dich auf dein Leben konzentrieren kannst.",
    footerCol1: "Für Einzelpersonen", footerCol2: "Für Unternehmen", footerCol3: "YouNomad",
    footerLinks1: ["YouNomad für dich", "Wie wir es machen", "Spanien 360°", "Loslegen"],
    footerLinks2: ["Für dein Talent", "Wie wir arbeiten", "Spanien 360°", "Plan gestalten"],
    footerLinks3: ["Über uns", "Eignungscheck", "Preise", "Kontakt"],
    footerCopy: "© 2026 YouNomad. Alle Rechte vorbehalten.",
    footerTagline: "Mit ♥ für globale Nomaden gemacht",
  },
  nl: {
    navPersons: "Voor particulieren", navCompanies: "Voor bedrijven",
    navDestinations: "Bestemmingen", navPricing: "Prijzen",
    navLogin: "Inloggen", navCta: "Aan de slag →",
    heroBadge: "Digitaal Nomaden Visum · Spanje",
    heroH1a: "Jouw infrastructuur om te", heroH1b: "leven", heroH1c: "en te", heroH1d: "werken",
    heroH1e: "in Spanje",
    heroSub: "Van je eerste visum tot volledige integratie. YouNomad verbindt visa's, vestigingsdiensten en community voor digitale nomaden, freelancers en internationale studenten.",
    heroCta1: "Geschiktheidscheck →", heroCta2: "Zie hoe het werkt",
    heroPill1: "Geen eindeloze telefoontjes", heroPill2: "Geen bureaucratisch drama", heroPill3: "Geen verloren papieren",
    trustLabel: "We werken samen met",
    srvLabel: "· Wat we doen ·",
    srvH2a: "Niet zomaar een visabureau,", srvH2b: "de ruggengraat van jouw", srvH2c: "globale leven",
    srvP: "Achter YouNomad staan juridische teams, mobiliteitsspeci­alisten en dienstverleners die jaarlijks duizenden internationale verhuizingen begeleiden. Één plek voor alles.",
    srv1Title: "Visa's & Regelgeving", srv1Desc: "We vergelijken officiële programma's voor nomaden, freelancers, studenten en remote teams. Lokale juridische ondersteuning en realtime tracking.",
    srv2Title: "Vestiging & Dagelijks Leven", srv2Desc: "Bankrekening, geverifieerde woning, zorgverzekering, lokale SIM, coworking en inschrijving. Alles essentiëls klaar vanaf dag één.",
    srv3Title: "Groei & Community", srv3Desc: "Evenementen, exclusieve voordelen, professionele middelen en echte verbindingen zodat jouw globale leven een langetermijnproject wordt.",
    srvMore: "Meer weten →",
    howLabel: "· Hoe het werkt ·",
    howH2a: "Een", howH2b: "doorlopende", howH2c: "reis",
    howP: "Dezelfde structuur begeleidt je van geschiktheid tot volledige integratie in Spanje.",
    step1Title: "Vertel ons wie je bent", step1Desc: "Deel je profiel en situatie. Geschiktheidscheck in minuten.",
    step2Title: "Stap-voor-stap begeleiding", step2Desc: "Documenten, tijdlijnen en gespecialiseerde juridische ondersteuning in elke fase.",
    step3Title: "We bereiden je aankomst voor", step3Desc: "Woning, bankrekening en SIM klaar voordat je in Spanje landt.",
    step4Title: "Bij je in je nieuwe leven", step4Desc: "Doorlopende ondersteuning, actieve community en professionele middelen vanaf dag één.",
    destLabel: "· Bestemmingen ·",
    destH2a: "Ontdek waar je wilt", destH2b: "leven", destH2c: "en", destH2d: "werken",
    destP: "De meest gekozen steden door de YouNomad-community in Spanje.",
    destMeta: ["Hoofdstad · Digitaal Nomade", "Tech · Middellandse Zee", "Cultuur · Uitzonderlijk Klimaat", "Strand · Startup Hub"],
    destTags: ["Meest populair", "Trending", "Groeiend", "Topaanbeveling"],
    stat1Label: "Voltooide verhuizingen", stat2Label: "Visa goedkeuringspercentage", stat3Label: "Hoofdbestemmingen in Spanje",
    testiLabel: "· Getuigenissen ·",
    testiH2a: "Wat mensen zeggen", testiH2b: "over ons",
    testiQuotes: [
      "Het visumproces leek onmogelijk totdat ik YouNomad vond. In minder dan een maand had ik alles klaar om naar Madrid te verhuizen. Ongelooflijk team.",
      "Ik had maanden geprobeerd de vereisten te begrijpen. YouNomad vereenvoudigde alles en het team was bij elke stap beschikbaar. Geen stress, geen verrassingen.",
      "Van de bankrekening tot het appartement in Barcelona, alles gecoördineerd. Ik voelde me vanaf de eerste dag begeleid. Absoluut aanbevolen.",
    ],
    testiRoles: ["UX-ontwerper · Mexico", "Ontwikkelaar · Ierland", "Consultant · Japan"],
    priceLabel: "· Prijzen ·",
    priceH2a: "Kies hoe je wilt dat", priceH2b: "YouNomad je begeleidt",
    priceP: "Geen verborgen kosten. Officiële tarieven en juridische vertegenwoordiging altijd gespecificeerd.",
    planNames: ["Visa Core", "Academisch + Visum", "Volledige Verhuizing"],
    planBadge: "Meest gekozen",
    planPeriod: "eenmalige vergoeding",
    planFeatures: [
      ["Geschikheidsquiz", "Vergelijking visaroutes", "Strategisch advies", "Volledige aanvraagvoorbereiding", "Documentenchecklist & review"],
      ["Alles in Visa Core", "Universiteitsselectie", "Aanvraagbeheer", "Coördinatie acceptatiebrief", "TIE-begeleiding bij aankomst"],
      ["Alles hierboven", "Geverifieerde woningondersteuning", "VS → Spanje-beheer", "TIE-begeleiding", "Prioritaire ondersteuning"],
    ],
    planCta: "Nu beginnen",
    ctaLabel: "· Begin Vandaag ·",
    ctaH2a: "Je globale leven begint", ctaH2b: "met één", ctaH2c: "beslissing",
    ctaP: "Deel je profiel. We begeleiden je door elke stap en bereiden je aankomst voor zodat je klaar bent om te leven, niet alleen formaliteiten te regelen.",
    ctaCta1: "Gratis geschiktheidscheck →", ctaCta2: "Praat met het team",
    footerDesc: "We vereenvoudigen het digitale visumproces voor nomaden in Spanje zodat je je kunt richten op het plannen van je leven.",
    footerCol1: "Voor particulieren", footerCol2: "Voor bedrijven", footerCol3: "YouNomad",
    footerLinks1: ["YouNomad voor jou", "Hoe we het doen", "Spanje 360°", "Aan de slag"],
    footerLinks2: ["Voor je talent", "Hoe we werken", "Spanje 360°", "Plan ontwerpen"],
    footerLinks3: ["Over ons", "Geschiktheidscheck", "Prijzen", "Contact"],
    footerCopy: "© 2026 YouNomad. Alle rechten voorbehouden.",
    footerTagline: "Gemaakt met ♥ voor globale nomaden",
  },
  sv: {
    navPersons: "För privatpersoner", navCompanies: "För företag",
    navDestinations: "Destinationer", navPricing: "Priser",
    navLogin: "Logga in", navCta: "Kom igång →",
    heroBadge: "Digitalt Nomadvisum · Spanien",
    heroH1a: "Din infrastruktur för att", heroH1b: "leva", heroH1c: "och", heroH1d: "arbeta",
    heroH1e: "i Spanien",
    heroSub: "Från ditt första visum till fullständig integration. YouNomad kopplar samman visum, bosättningstjänster och gemenskap för digitala nomader, frilansare och internationella studenter.",
    heroCta1: "Behörighetskoll →", heroCta2: "Se hur det fungerar",
    heroPill1: "Inga ändlösa samtal", heroPill2: "Inget byråkratiskt drama", heroPill3: "Inga förlorade dokument",
    trustLabel: "Vi samarbetar med",
    srvLabel: "· Vad vi gör ·",
    srvH2a: "Inte bara en visumbyå,", srvH2b: "ryggraden i ditt", srvH2c: "globala liv",
    srvP: "Bakom YouNomad finns juridiska team, mobilitetsspecialister och tjänsteleverantörer som hjälper tusentals internationella flytt varje år. En plats för allt.",
    srv1Title: "Visum & Reglering", srv1Desc: "Vi jämför officiella program för nomader, frilansare, studenter och remote-team. Lokalt juridiskt stöd och realtidsspårning.",
    srv2Title: "Etablering & Vardag", srv2Desc: "Bankkonto, verifierat boende, sjukförsäkring, lokalt SIM, coworking och registrering. Allt väsentligt klart från dag ett.",
    srv3Title: "Tillväxt & Gemenskap", srv3Desc: "Evenemang, exklusiva förmåner, professionella resurser och riktiga kontakter så att ditt globala liv blir ett långsiktigt projekt.",
    srvMore: "Läs mer →",
    howLabel: "· Hur det fungerar ·",
    howH2a: "En", howH2b: "kontinuerlig", howH2c: "resa",
    howP: "Samma struktur vägleder dig från behörighet till fullständig integration i Spanien.",
    step1Title: "Berätta vem du är", step1Desc: "Dela din profil och situation. Behörighetskoll på några minuter.",
    step2Title: "Steg-för-steg-vägledning", step2Desc: "Dokument, tidslinjer och specialiserat juridiskt stöd i varje steg.",
    step3Title: "Vi förbereder din ankomst", step3Desc: "Boende, bankkonto och SIM klart innan du landar i Spanien.",
    step4Title: "Med dig i ditt nya liv", step4Desc: "Löpande stöd, aktiv gemenskap och professionella resurser från dag ett.",
    destLabel: "· Destinationer ·",
    destH2a: "Utforska var du vill", destH2b: "leva", destH2c: "och", destH2d: "arbeta",
    destP: "De mest valda städerna av YouNomad-gemenskapen i Spanien.",
    destMeta: ["Huvudstad · Digital Nomad", "Tech · Medelhavet", "Kultur · Enastående Klimat", "Strand · Startup Hub"],
    destTags: ["Mest populär", "Trending", "Växande", "Toppval"],
    stat1Label: "Genomförda flytt", stat2Label: "Visumsbeviljandegrad", stat3Label: "Huvuddestinationer i Spanien",
    testiLabel: "· Vittnesmål ·",
    testiH2a: "Vad folk säger", testiH2b: "om oss",
    testiQuotes: [
      "Visumprocessen verkade omöjlig tills jag hittade YouNomad. På mindre än en månad var allt klart för flytten till Madrid. Otroligt team.",
      "Jag hade spenderat månader på att försöka förstå kraven. YouNomad förenklade allt och teamet var tillgängligt i varje steg. Ingen stress, inga överraskningar.",
      "Från bankkontot till lägenheten i Barcelona – allt koordinerat. Jag kände mig stöttad från dag ett. Absolut rekommenderas.",
    ],
    testiRoles: ["UX-designer · Mexiko", "Utvecklare · Irland", "Konsult · Japan"],
    priceLabel: "· Priser ·",
    priceH2a: "Välj hur du vill att", priceH2b: "YouNomad stöttar dig",
    priceP: "Inga dolda kostnader. Officiella avgifter och juridisk representation alltid specificerade.",
    planNames: ["Visa Core", "Akademisk + Visum", "Fullständig Flytt"],
    planBadge: "Mest vald",
    planPeriod: "engångsavgift",
    planFeatures: [
      ["Behörighetsquiz", "Jämförelse av visumsvägar", "Strategisk konsultation", "Fullständig ansökansberedning", "Dokumentchecklista & granskning"],
      ["Allt i Visa Core", "Universitetsval", "Ansökningshantering", "Koordination av antagningsbesked", "TIE-ledsagning vid ankomst"],
      ["Allt ovan", "Verifierat bostadsstöd", "USA → Spanien-hantering", "TIE-ledsagning", "Prioriterat stöd genomgående"],
    ],
    planCta: "Kom igång nu",
    ctaLabel: "· Börja Idag ·",
    ctaH2a: "Ditt globala liv börjar", ctaH2b: "med ett", ctaH2c: "beslut",
    ctaP: "Dela din profil. Vi guidar dig genom varje steg och förbereder din ankomst så att du är redo att leva, inte bara göra pappersarbete.",
    ctaCta1: "Gratis behörighetskoll →", ctaCta2: "Prata med teamet",
    footerDesc: "Vi förenklar det digitala visumprocessen för nomader i Spanien så att du kan fokusera på att planera ditt liv.",
    footerCol1: "För privatpersoner", footerCol2: "För företag", footerCol3: "YouNomad",
    footerLinks1: ["YouNomad för dig", "Hur vi gör det", "Spanien 360°", "Kom igång"],
    footerLinks2: ["För din talang", "Hur vi arbetar", "Spanien 360°", "Designa en plan"],
    footerLinks3: ["Om oss", "Behörighetskoll", "Priser", "Kontakt"],
    footerCopy: "© 2026 YouNomad. Alla rättigheter förbehållna.",
    footerTagline: "Gjord med ♥ för globala nomader",
  },
  da: {
    navPersons: "For enkeltpersoner", navCompanies: "For virksomheder",
    navDestinations: "Destinationer", navPricing: "Priser",
    navLogin: "Log ind", navCta: "Kom i gang →",
    heroBadge: "Digitalt Nomadevisum · Spanien",
    heroH1a: "Din infrastruktur til at", heroH1b: "leve", heroH1c: "og", heroH1d: "arbejde",
    heroH1e: "i Spanien",
    heroSub: "Fra dit første visum til fuld integration. YouNomad forbinder visa, bosættelsestjenester og fællesskab for digitale nomader, freelancere og internationale studerende.",
    heroCta1: "Egnetheds­tjek →", heroCta2: "Se hvordan det virker",
    heroPill1: "Ingen endeløse opkald", heroPill2: "Intet bureaukratisk drama", heroPill3: "Ingen tabte dokumenter",
    trustLabel: "Vi arbejder med",
    srvLabel: "· Hvad vi gør ·",
    srvH2a: "Ikke endnu et visumbureau,", srvH2b: "rygraden i dit", srvH2c: "globale liv",
    srvP: "Bag YouNomad er juridiske teams, mobilitetsspecialister og tjenesteudbydere, der ledsager tusindvis af internationale flytninger hvert år. Ét sted for alt.",
    srv1Title: "Visa & Regulering", srv1Desc: "Vi sammenligner officielle programmer for nomader, freelancere, studerende og remote-teams. Lokal juridisk støtte og realtidssporing.",
    srv2Title: "Bosættelse & Hverdag", srv2Desc: "Bankkonto, verificeret bolig, sygeforsikring, lokalt SIM, coworking og registrering. Alt det væsentlige klar fra dag ét.",
    srv3Title: "Vækst & Fællesskab", srv3Desc: "Events, eksklusive fordele, professionelle ressourcer og ægte forbindelser, så dit globale liv bliver et langsigtet projekt.",
    srvMore: "Lær mere →",
    howLabel: "· Sådan virker det ·",
    howH2a: "En", howH2b: "kontinuerlig", howH2c: "rejse",
    howP: "Samme struktur guider dig fra egnethed til fuld integration i Spanien.",
    step1Title: "Fortæl os om dig selv", step1Desc: "Del din profil og situation. Egnethedstjek på få minutter.",
    step2Title: "Trin-for-trin vejledning", step2Desc: "Dokumenter, tidslinjer og specialiseret juridisk støtte i hvert trin.",
    step3Title: "Vi forbereder din ankomst", step3Desc: "Bolig, bankkonto og SIM klar inden du lander i Spanien.",
    step4Title: "Med dig i dit nye liv", step4Desc: "Løbende støtte, aktivt fællesskab og professionelle ressourcer fra dag ét.",
    destLabel: "· Destinationer ·",
    destH2a: "Udforsk hvor du vil", destH2b: "leve", destH2c: "og", destH2d: "arbejde",
    destP: "De mest valgte byer af YouNomad-fællesskabet i Spanien.",
    destMeta: ["Hovedstad · Digital Nomade", "Tech · Middelhavet", "Kultur · Exceptionelt Klima", "Strand · Startup Hub"],
    destTags: ["Mest populær", "Trending", "Voksende", "Topvalg"],
    stat1Label: "Gennemførte flytninger", stat2Label: "Visa godkendelsesrate", stat3Label: "Hoveddestinationer i Spanien",
    testiLabel: "· Anmeldelser ·",
    testiH2a: "Hvad folk siger", testiH2b: "om os",
    testiQuotes: [
      "Visumprocessen virkede umulig, indtil jeg fandt YouNomad. På under en måned var alt klar til flytningen til Madrid. Utroligt team.",
      "Jeg havde brugt måneder på at forsøge at forstå kravene. YouNomad forenklede alt, og teamet var tilgængeligt ved hvert trin. Ingen stress, ingen overraskelser.",
      "Fra bankkontoen til lejligheden i Barcelona – alt koordineret. Jeg følte mig støttet fra dag ét. Absolut anbefalet.",
    ],
    testiRoles: ["UX-designer · Mexico", "Udvikler · Irland", "Konsulent · Japan"],
    priceLabel: "· Priser ·",
    priceH2a: "Vælg hvordan du vil have at", priceH2b: "YouNomad støtter dig",
    priceP: "Ingen skjulte omkostninger. Officielle gebyrer og juridisk repræsentation altid specificeret.",
    planNames: ["Visa Core", "Akademisk + Visum", "Fuld Flytning"],
    planBadge: "Mest valgt",
    planPeriod: "engangsgebyr",
    planFeatures: [
      ["Egnetheds­quiz", "Sammenligning af visum­ruter", "Strategisk konsultation", "Fuld ansøgningsforberedelse", "Dokument­tjekliste & gennemgang"],
      ["Alt i Visa Core", "Universitetsvalg", "Ansøgningshåndtering", "Koordination af optagelsesbrev", "TIE-ledsagelse ved ankomst"],
      ["Alt ovenstående", "Verificeret boligs­tøtte", "USA → Spanien-håndtering", "TIE-ledsagelse", "Prioriteret støtte hele vejen"],
    ],
    planCta: "Kom i gang nu",
    ctaLabel: "· Start I Dag ·",
    ctaH2a: "Dit globale liv starter", ctaH2b: "med én", ctaH2c: "beslutning",
    ctaP: "Del din profil. Vi guider dig gennem hvert trin og forbereder din ankomst, så du er klar til at leve, ikke bare gøre papirarbejde.",
    ctaCta1: "Gratis egnethedstjek →", ctaCta2: "Tal med teamet",
    footerDesc: "Vi forenkler den digitale visumproces for nomader i Spanien, så du kan fokusere på at planlægge dit liv.",
    footerCol1: "For enkeltpersoner", footerCol2: "For virksomheder", footerCol3: "YouNomad",
    footerLinks1: ["YouNomad for dig", "Sådan gør vi det", "Spanien 360°", "Kom i gang"],
    footerLinks2: ["For dit talent", "Sådan arbejder vi", "Spanien 360°", "Design en plan"],
    footerLinks3: ["Om os", "Egnethedstjek", "Priser", "Kontakt"],
    footerCopy: "© 2026 YouNomad. Alle rettigheder forbeholdt.",
    footerTagline: "Lavet med ♥ til globale nomader",
  },
  no: {
    navPersons: "For enkeltpersoner", navCompanies: "For bedrifter",
    navDestinations: "Destinasjoner", navPricing: "Priser",
    navLogin: "Logg inn", navCta: "Kom i gang →",
    heroBadge: "Digitalt Nomadevisum · Spania",
    heroH1a: "Din infrastruktur for å", heroH1b: "leve", heroH1c: "og", heroH1d: "jobbe",
    heroH1e: "i Spania",
    heroSub: "Fra ditt første visum til full integrering. YouNomad kobler visum, bosettingstjenester og fellesskap for digitale nomader, frilansere og internasjonale studenter.",
    heroCta1: "Egnethetsjekk →", heroCta2: "Se hvordan det fungerer",
    heroPill1: "Ingen endeløse samtaler", heroPill2: "Ingen byråkratisk drama", heroPill3: "Ingen tapte dokumenter",
    trustLabel: "Vi samarbeider med",
    srvLabel: "· Hva vi gjør ·",
    srvH2a: "Ikke bare et visumskontor,", srvH2b: "ryggraden i ditt", srvH2c: "globale liv",
    srvP: "Bak YouNomad er juridiske team, mobilitetseksperter og tjenesteleverandører som følger opp tusenvis av internasjonale flyttinger hvert år. Ett sted for alt.",
    srv1Title: "Visum & Regulering", srv1Desc: "Vi sammenligner offisielle programmer for nomader, frilansere, studenter og remote-team. Lokal juridisk støtte og sanntidssporing.",
    srv2Title: "Etablering & Hverdag", srv2Desc: "Bankkonto, verifisert bolig, helseforsikring, lokalt SIM, coworking og registrering. Alt essensielt klart fra dag én.",
    srv3Title: "Vekst & Fellesskap", srv3Desc: "Arrangementer, eksklusive fordeler, profesjonelle ressurser og ekte forbindelser slik at ditt globale liv blir et langsiktig prosjekt.",
    srvMore: "Lær mer →",
    howLabel: "· Slik fungerer det ·",
    howH2a: "En", howH2b: "kontinuerlig", howH2c: "reise",
    howP: "Samme struktur veileder deg fra egnethet til full integrering i Spania.",
    step1Title: "Fortell oss hvem du er", step1Desc: "Del profilen og situasjonen din. Egnethetsjekk på noen minutter.",
    step2Title: "Steg-for-steg-veiledning", step2Desc: "Dokumenter, tidslinjer og spesialisert juridisk støtte i hvert steg.",
    step3Title: "Vi forbereder ankomsten din", step3Desc: "Bolig, bankkonto og SIM klart før du lander i Spania.",
    step4Title: "Med deg i ditt nye liv", step4Desc: "Løpende støtte, aktivt fellesskap og profesjonelle ressurser fra dag én.",
    destLabel: "· Destinasjoner ·",
    destH2a: "Utforsk hvor du vil", destH2b: "leve", destH2c: "og", destH2d: "jobbe",
    destP: "De mest valgte byene av YouNomad-fellesskapet i Spania.",
    destMeta: ["Hovedstad · Digital Nomade", "Tech · Middelhavet", "Kultur · Eksepsjonelt Klima", "Strand · Startup Hub"],
    destTags: ["Mest populær", "Trending", "Voksende", "Toppvalg"],
    stat1Label: "Gjennomførte flyttinger", stat2Label: "Visumgodkjenningsrate", stat3Label: "Hoveddestinasoner i Spania",
    testiLabel: "· Anmeldelser ·",
    testiH2a: "Hva folk sier", testiH2b: "om oss",
    testiQuotes: [
      "Visumprosessen virket umulig til jeg fant YouNomad. På under en måned var alt klart for flyttingen til Madrid. Utrolig team.",
      "Jeg hadde brukt måneder på å prøve å forstå kravene. YouNomad forenklet alt og teamet var tilgjengelig i hvert steg. Ingen stress, ingen overraskelser.",
      "Fra bankkontoen til leiligheten i Barcelona – alt koordinert. Jeg følte meg ivaretatt fra dag én. Absolutt anbefalt.",
    ],
    testiRoles: ["UX-designer · Mexico", "Utvikler · Irland", "Konsulent · Japan"],
    priceLabel: "· Priser ·",
    priceH2a: "Velg hvordan du vil at", priceH2b: "YouNomad støtter deg",
    priceP: "Ingen skjulte kostnader. Offisielle avgifter og juridisk representasjon alltid spesifisert.",
    planNames: ["Visa Core", "Akademisk + Visum", "Full Flytting"],
    planBadge: "Mest valgt",
    planPeriod: "engangsgebyr",
    planFeatures: [
      ["Egnethetsjekk-quiz", "Sammenligning av visum­ruter", "Strategisk konsultasjon", "Full søknads­forberedelse", "Dokumentsjekkliste & gjennomgang"],
      ["Alt i Visa Core", "Universitetsvalg", "Søknadshåndtering", "Koordinasjon av opptaksbrev", "TIE-ledsaging ved ankomst"],
      ["Alt ovenfor", "Verifisert boligstøtte", "USA → Spania-håndtering", "TIE-ledsaging", "Prioritert støtte hele veien"],
    ],
    planCta: "Kom i gang nå",
    ctaLabel: "· Start I Dag ·",
    ctaH2a: "Ditt globale liv starter", ctaH2b: "med én", ctaH2c: "beslutning",
    ctaP: "Del profilen din. Vi veileder deg gjennom hvert steg og forbereder ankomsten din slik at du er klar til å leve, ikke bare gjøre papirarbeid.",
    ctaCta1: "Gratis egnethetsjekk →", ctaCta2: "Snakk med teamet",
    footerDesc: "Vi forenkler den digitale visumprosessen for nomader i Spania slik at du kan fokusere på å planlegge livet ditt.",
    footerCol1: "For enkeltpersoner", footerCol2: "For bedrifter", footerCol3: "YouNomad",
    footerLinks1: ["YouNomad for deg", "Slik gjør vi det", "Spania 360°", "Kom i gang"],
    footerLinks2: ["For ditt talent", "Slik jobber vi", "Spania 360°", "Design en plan"],
    footerLinks3: ["Om oss", "Egnethetsjekk", "Priser", "Kontakt"],
    footerCopy: "© 2026 YouNomad. Alle rettigheter forbeholdt.",
    footerTagline: "Laget med ♥ for globale nomader",
  },
  fi: {
    navPersons: "Yksityishenkilöille", navCompanies: "Yrityksille",
    navDestinations: "Kohteet", navPricing: "Hinnat",
    navLogin: "Kirjaudu", navCta: "Aloita →",
    heroBadge: "Digitaalinen Nomaadiviisumi · Espanja",
    heroH1a: "Infrastruktuurisi", heroH1b: "elää", heroH1c: "ja", heroH1d: "työskennellä",
    heroH1e: "Espanjassa",
    heroSub: "Ensimmäisestä viisumista täydelliseen integraatioon. YouNomad yhdistää viisumit, maahantulo­palvelut ja yhteisön digitaalisille nomaadeille, freelancereille ja kansainvälisille opiskelijoille.",
    heroCta1: "Soveltuvuustarkistus →", heroCta2: "Katso miten se toimii",
    heroPill1: "Ei loputtomia puheluita", heroPill2: "Ei byrokraattista draamaa", heroPill3: "Ei kadonneita papereita",
    trustLabel: "Teemme yhteistyötä",
    srvLabel: "· Mitä teemme ·",
    srvH2a: "Ei vain toinen viisumitoimisto,", srvH2b: "globaalin elämäsi", srvH2c: "selkäranka",
    srvP: "YouNomadin takana on juridisia tiimejä, liikkuvuusasiantuntijoita ja palveluntarjoajia, jotka auttavat tuhansia kansainvälisiä muuttoja vuosittain. Yksi paikka kaikelle.",
    srv1Title: "Viisumit & Säädökset", srv1Desc: "Vertaamme virallisia ohjelmia nomaadeille, freelancereille, opiskelijoille ja etätiimeille. Paikallinen juridinen tuki ja reaaliaikainen seuranta.",
    srv2Title: "Maahantulo & Arki", srv2Desc: "Pankkitili, varmennettu asunto, sairausvakuutus, paikallinen SIM, coworking ja rekisteröinti. Kaikki olennainen valmiina ensimmäisestä päivästä.",
    srv3Title: "Kasvu & Yhteisö", srv3Desc: "Tapahtumat, eksklusiiviset edut, ammatilliset resurssit ja aidot yhteydet, jotta globaali elämäsi on pitkäaikainen projekti.",
    srvMore: "Lue lisää →",
    howLabel: "· Miten se toimii ·",
    howH2a: "Jatkuva", howH2b: "matka", howH2c: "",
    howP: "Sama rakenne ohjaa sinut soveltuvuudesta täydelliseen integraatioon Espanjassa.",
    step1Title: "Kerro meille kuka olet", step1Desc: "Jaa profiilisi ja tilanteesi. Soveltuvuustarkistus muutamassa minuutissa.",
    step2Title: "Askel askeleelta -opas", step2Desc: "Asiakirjat, aikataulut ja erikoistunut juridinen tuki jokaisessa vaiheessa.",
    step3Title: "Valmistamme saapumisesi", step3Desc: "Asunto, pankkitili ja SIM valmiina ennen kuin laskeutut Espanjaan.",
    step4Title: "Kanssasi uudessa elämässä", step4Desc: "Jatkuva tuki, aktiivinen yhteisö ja ammatilliset resurssit ensimmäisestä päivästä.",
    destLabel: "· Kohteet ·",
    destH2a: "Tutki missä haluat", destH2b: "elää", destH2c: "ja", destH2d: "työskennellä",
    destP: "YouNomad-yhteisön suosituimmat kaupungit Espanjassa.",
    destMeta: ["Pääkaupunki · Digitaalinen Nomaadi", "Tech · Välimeri", "Kulttuuri · Poikkeuksellinen Ilmasto", "Ranta · Startup Hub"],
    destTags: ["Suosituin", "Trending", "Kasvava", "Parras­valinta"],
    stat1Label: "Valmistuneet muutot", stat2Label: "Viisumin hyväksymisprosentti", stat3Label: "Pääkohteet Espanjassa",
    testiLabel: "· Arvostelut ·",
    testiH2a: "Mitä ihmiset sanovat", testiH2b: "meistä",
    testiQuotes: [
      "Viisumiprosessi tuntui mahdottomalta, kunnes löysin YouNomadin. Alle kuukaudessa kaikki oli valmista muuttoa varten Madridiin. Uskomaton tiimi.",
      "Olin viettänyt kuukausia yrittäessäni ymmärtää vaatimuksia. YouNomad yksinkertaisti kaiken ja tiimi oli käytettävissä jokaisessa vaiheessa. Ei stressiä, ei yllätyksiä.",
      "Pankkitilistä asuntoon Barcelonassa – kaikki koordinoitu. Tunsin itseni tuetuksi ensimmäisestä päivästä. Suosittelen ehdottomasti.",
    ],
    testiRoles: ["UX-suunnittelija · Meksiko", "Kehittäjä · Irlanti", "Konsultti · Japani"],
    priceLabel: "· Hinnat ·",
    priceH2a: "Valitse miten haluat", priceH2b: "YouNomadin tukevan sinua",
    priceP: "Ei piilotettuja kuluja. Viralliset maksut ja oikeudellinen edustus aina eriteltyinä.",
    planNames: ["Visa Core", "Akateeminen + Viisumi", "Täydellinen Muutto"],
    planBadge: "Suosituin",
    planPeriod: "kertamaksu",
    planFeatures: [
      ["Soveltuvuusquiz", "Viisumireittien vertailu", "Strateginen konsultaatio", "Täydellinen hakemuksen valmistelu", "Asiakirjatarkistuslista & arviointi"],
      ["Kaikki Visa Coressa", "Yliopistovalinta", "Hakemustenhallinta", "Hyväksymiskirjeen koordinointi", "TIE-saattaminen saapumisessa"],
      ["Kaikki yllä mainittu", "Varmennettu asuntotuki", "USA → Espanja -hallinta", "TIE-saattaminen", "Prioriteettituki läpi prosessin"],
    ],
    planCta: "Aloita nyt",
    ctaLabel: "· Aloita Tänään ·",
    ctaH2a: "Globaali elämäsi alkaa", ctaH2b: "yhdellä", ctaH2c: "päätöksellä",
    ctaP: "Jaa profiilisi. Ohjaamme sinut jokaisen vaiheen läpi ja valmistamme saapumisesi, jotta olet valmis elämään, ei vain tekemään paperitöitä.",
    ctaCta1: "Ilmainen soveltuvuustarkistus →", ctaCta2: "Puhu tiimin kanssa",
    footerDesc: "Yksinkertaistamme digitaalisen viisumiprosessin nomaadeille Espanjassa, jotta voit keskittyä elämäsi suunnitteluun.",
    footerCol1: "Yksityishenkilöille", footerCol2: "Yrityksille", footerCol3: "YouNomad",
    footerLinks1: ["YouNomad sinulle", "Miten teemme sen", "Espanja 360°", "Aloita"],
    footerLinks2: ["Talentillesi", "Miten työskentelemme", "Espanja 360°", "Suunnittele suunnitelma"],
    footerLinks3: ["Tietoa meistä", "Soveltuvuustarkistus", "Hinnat", "Yhteystiedot"],
    footerCopy: "© 2026 YouNomad. Kaikki oikeudet pidätetään.",
    footerTagline: "Tehty ♥ globaaleille nomaadeille",
  },
};

/* ─── CSS ───────────────────────────────────────────────────────────────── */
const css = `
  @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700&family=Bricolage+Grotesque:wght@400;500;600;700;800&display=swap');

  *, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }
  html { scroll-behavior:smooth; }
  body {
    background:var(--bg); color:var(--fg);
    font-family:'Plus Jakarta Sans',sans-serif; font-size:15px; line-height:1.6;
    -webkit-font-smoothing:antialiased; overflow-x:hidden;
    transition:background 0.35s,color 0.35s;
  }

  /* NAV */
  nav {
    position:fixed; top:0; left:0; right:0; z-index:200;
    display:flex; align-items:center; justify-content:space-between;
    padding:16px 48px;
    background:color-mix(in srgb,var(--bg) 88%,transparent);
    backdrop-filter:blur(16px);
    border-bottom:1px solid var(--border-dim);
    transition:padding 0.3s,background 0.35s,border-color 0.35s;
    gap:16px;
  }
  nav.compact { padding:10px 48px; }

  .logo {
    display:flex; align-items:center; gap:10px;
    font-family:'Bricolage Grotesque',sans-serif;
    font-size:1.2rem; font-weight:700;
    color:var(--fg); text-decoration:none; letter-spacing:-0.02em; flex-shrink:0;
  }
  .logo-mark {
    width:26px; height:26px; border-radius:7px;
    background:var(--lime); display:flex; align-items:center; justify-content:center;
    box-shadow:0 0 12px var(--lime-glow);
    transition:background 0.35s,box-shadow 0.35s; flex-shrink:0;
  }
  .logo-mark-inner { width:9px; height:9px; border-radius:50%; background:#0D0E0F; }

  .nav-center { display:flex; gap:2px; align-items:center; }
  .nav-center a {
    text-decoration:none; color:var(--fg2);
    font-size:0.84rem; font-weight:500;
    padding:6px 12px; border-radius:8px;
    transition:color 0.2s,background 0.2s; white-space:nowrap;
  }
  .nav-center a:hover { color:var(--fg); background:var(--card); }

  .nav-right { display:flex; gap:8px; align-items:center; flex-shrink:0; }

  /* ── LANG PICKER ── */
  .lang-picker { position:relative; }
  .lang-btn {
    display:flex; align-items:center; gap:6px;
    background:var(--card); border:1px solid var(--border);
    color:var(--fg); padding:7px 12px; border-radius:8px;
    font-size:0.83rem; font-weight:600; cursor:pointer;
    font-family:'Plus Jakarta Sans',sans-serif;
    transition:border-color 0.2s,box-shadow 0.2s;
    white-space:nowrap; user-select:none;
  }
  .lang-btn:hover { border-color:var(--blue); box-shadow:0 0 10px var(--blue-glow); }
  .lang-btn .chevron {
    font-size:0.6rem; color:var(--fg3); margin-left:2px;
    transition:transform 0.2s;
  }
  .lang-btn.open .chevron { transform:rotate(180deg); }

  .lang-dropdown {
    position:absolute; top:calc(100% + 8px); right:0;
    background:var(--card); border:1px solid var(--border);
    border-radius:12px; overflow:hidden;
    box-shadow:0 20px 48px rgba(0,0,0,0.3);
    min-width:180px;
    animation:dropIn 0.18s ease;
    z-index:300;
  }
  @keyframes dropIn {
    from { opacity:0; transform:translateY(-8px); }
    to   { opacity:1; transform:translateY(0); }
  }
  .lang-option {
    display:flex; align-items:center; gap:10px;
    padding:10px 16px; cursor:pointer;
    font-size:0.86rem; font-weight:500; color:var(--fg2);
    transition:background 0.15s, color 0.15s;
    border-bottom:1px solid var(--border-dim);
  }
  .lang-option:last-child { border-bottom:none; }
  .lang-option:hover { background:var(--surface); color:var(--fg); }
  .lang-option.active { color:var(--blue); background:var(--blue-glow); }
  .lang-option .lang-flag { font-size:1.15rem; flex-shrink:0; }
  .lang-option .lang-name { flex:1; }
  .lang-option .lang-check { color:var(--blue); font-size:0.75rem; }

  /* MODE TOGGLE */
  .mode-toggle {
    width:38px; height:38px; border-radius:9px;
    background:var(--card); border:1px solid var(--border);
    display:flex; align-items:center; justify-content:center;
    cursor:pointer; font-size:0.95rem;
    transition:all 0.2s; flex-shrink:0;
  }
  .mode-toggle:hover { border-color:var(--blue); box-shadow:0 0 12px var(--blue-glow); }

  .btn-ghost {
    background:none; border:1px solid var(--border);
    color:var(--fg); padding:7px 16px; border-radius:8px;
    font-size:0.83rem; font-weight:500; cursor:pointer;
    font-family:'Plus Jakarta Sans',sans-serif;
    transition:border-color 0.2s,color 0.2s;
    text-decoration:none; display:inline-block; white-space:nowrap;
  }
  .btn-ghost:hover { border-color:var(--blue); color:var(--blue); }

  .btn-lime {
    background:var(--lime); color:#0D0E0F;
    border:none; padding:8px 18px; border-radius:8px;
    font-size:0.83rem; font-weight:700; cursor:pointer;
    font-family:'Plus Jakarta Sans',sans-serif;
    transition:background 0.2s,box-shadow 0.2s;
    text-decoration:none; display:inline-block; white-space:nowrap;
  }
  .btn-lime:hover { background:var(--lime-dim); box-shadow:0 0 20px var(--lime-glow); }

  /* HERO */
  .hero {
    min-height:100vh; display:flex; flex-direction:column;
    align-items:center; justify-content:center;
    text-align:center; padding:140px 48px 80px;
    position:relative; overflow:hidden;
  }
  .hero-glow-lime {
    position:absolute; top:-150px; left:30%; transform:translateX(-50%);
    width:600px; height:600px; border-radius:50%;
    background:radial-gradient(circle,var(--lime-glow) 0%,transparent 70%);
    pointer-events:none;
  }
  .hero-glow-blue {
    position:absolute; top:-100px; right:15%;
    width:500px; height:500px; border-radius:50%;
    background:radial-gradient(circle,var(--blue-glow) 0%,transparent 70%);
    pointer-events:none;
  }
  .hero-grid {
    position:absolute; inset:0;
    background-image:
      linear-gradient(var(--border-dim) 1px,transparent 1px),
      linear-gradient(90deg,var(--border-dim) 1px,transparent 1px);
    background-size:60px 60px;
    mask-image:radial-gradient(ellipse 80% 60% at 50% 50%,black 30%,transparent 100%);
    opacity:var(--hero-grid-opacity);
    pointer-events:none; transition:opacity 0.35s;
  }
  .hero-badge {
    display:inline-flex; align-items:center; gap:8px;
    background:var(--card); border:1px solid var(--border);
    padding:6px 14px; border-radius:100px;
    font-size:0.75rem; font-weight:600; letter-spacing:0.07em; text-transform:uppercase;
    color:var(--lime); margin-bottom:32px; position:relative;
    transition:background 0.35s,border-color 0.35s;
  }
  .badge-dot {
    width:6px; height:6px; border-radius:50%;
    background:var(--lime); animation:blink 2s ease-in-out infinite;
    box-shadow:0 0 6px var(--lime);
  }
  @keyframes blink { 0%,100%{opacity:1;} 50%{opacity:0.3;} }

  .hero h1 {
    font-family:'Bricolage Grotesque',sans-serif;
    font-size:clamp(2.8rem,6vw,6.5rem);
    font-weight:800; line-height:1.0; letter-spacing:-0.03em;
    color:var(--fg); max-width:900px; margin-bottom:24px; position:relative;
    transition:color 0.35s;
  }
  .al { color:var(--lime); }
  .ab { color:var(--blue); }

  .hero-sub {
    font-size:1.05rem; color:var(--fg2); max-width:560px; line-height:1.7;
    margin-bottom:44px; position:relative; transition:color 0.35s;
  }
  .hero-actions {
    display:flex; gap:12px; align-items:center;
    flex-wrap:wrap; justify-content:center; margin-bottom:60px; position:relative;
  }
  .btn-lime-lg {
    background:var(--lime); color:#0D0E0F; border:none;
    padding:13px 30px; border-radius:10px;
    font-size:0.93rem; font-weight:700; cursor:pointer;
    font-family:'Plus Jakarta Sans',sans-serif;
    display:inline-flex; align-items:center; gap:8px;
    transition:all 0.25s; text-decoration:none;
  }
  .btn-lime-lg:hover { background:var(--lime-dim); box-shadow:0 0 30px var(--lime-glow); transform:translateY(-1px); }
  .btn-blue-lg {
    background:var(--blue); color:#fff; border:none;
    padding:13px 26px; border-radius:10px;
    font-size:0.93rem; font-weight:600; cursor:pointer;
    font-family:'Plus Jakarta Sans',sans-serif;
    display:inline-flex; align-items:center; gap:8px;
    transition:all 0.25s; text-decoration:none;
  }
  .btn-blue-lg:hover { background:var(--blue-dim); box-shadow:0 0 24px var(--blue-glow); transform:translateY(-1px); }

  .hero-pills { display:flex; gap:24px; flex-wrap:wrap; justify-content:center; position:relative; }
  .hero-pill { display:flex; align-items:center; gap:6px; font-size:0.81rem; color:var(--fg2); }
  .pill-x { color:var(--fg3); font-size:0.7rem; }

  /* SECTIONS */
  .section { padding:100px 48px; }
  .section-label { font-size:0.72rem; font-weight:700; letter-spacing:0.12em; text-transform:uppercase; margin-bottom:16px; }
  .section-label.lime { color:var(--lime); }
  .section-label.blue { color:var(--blue); }
  .section-h2 {
    font-family:'Bricolage Grotesque',sans-serif;
    font-size:clamp(1.9rem,3.5vw,3.2rem); font-weight:800; line-height:1.1;
    letter-spacing:-0.025em; color:var(--fg); margin-bottom:16px;
    transition:color 0.35s;
  }
  .section-p { font-size:1rem; color:var(--fg2); line-height:1.7; max-width:520px; transition:color 0.35s; }

  /* TRUST */
  .trust-bar {
    padding:26px 48px; border-top:1px solid var(--border-dim); border-bottom:1px solid var(--border-dim);
    display:flex; align-items:center; gap:40px; flex-wrap:wrap; transition:border-color 0.35s;
  }
  .trust-label { font-size:0.76rem; color:var(--fg3); white-space:nowrap; font-weight:500; letter-spacing:0.06em; text-transform:uppercase; }
  .trust-logos { display:flex; gap:36px; align-items:center; flex-wrap:wrap; }
  .trust-logo { font-size:0.83rem; font-weight:700; color:var(--fg3); letter-spacing:0.04em; text-transform:uppercase; opacity:0.55; transition:opacity 0.2s,color 0.2s; cursor:default; }
  .trust-logo:hover { opacity:1; color:var(--fg); }

  /* SERVICES */
  .services-section { background:var(--surface); transition:background 0.35s; }
  .services-header { display:grid; grid-template-columns:1fr 1fr; gap:60px; margin-bottom:64px; align-items:end; }
  .services-grid { display:grid; grid-template-columns:repeat(3,1fr); gap:2px; background:var(--border-dim); border-radius:16px; overflow:hidden; transition:background 0.35s; }
  .srv-card { background:var(--card); padding:40px 36px; position:relative; overflow:hidden; transition:background 0.3s; }
  .srv-card:hover { background:var(--surface); }
  .srv-card::after { content:''; position:absolute; bottom:0; left:0; right:0; height:2px; transform:scaleX(0); transform-origin:left; transition:transform 0.35s; }
  .srv-card:nth-child(1)::after { background:var(--lime); }
  .srv-card:nth-child(2)::after { background:var(--blue); }
  .srv-card:nth-child(3)::after { background:var(--lime); }
  .srv-card:hover::after { transform:scaleX(1); }
  .srv-icon { width:48px; height:48px; border-radius:12px; display:flex; align-items:center; justify-content:center; font-size:1.4rem; margin-bottom:24px; transition:background 0.35s,border-color 0.35s; }
  .srv-icon.lime-icon { background:var(--lime-glow); border:1px solid rgba(200,241,53,0.2); }
  .srv-icon.blue-icon { background:var(--blue-glow); border:1px solid rgba(75,123,245,0.2); }
  .srv-num { position:absolute; top:28px; right:32px; font-family:'Bricolage Grotesque',sans-serif; font-size:3.5rem; font-weight:800; color:var(--border-dim); line-height:1; transition:color 0.35s; }
  .srv-title { font-family:'Bricolage Grotesque',sans-serif; font-size:1.1rem; font-weight:700; color:var(--fg); margin-bottom:12px; line-height:1.2; transition:color 0.35s; }
  .srv-desc { font-size:0.85rem; color:var(--fg2); line-height:1.65; transition:color 0.35s; }
  .srv-link { display:inline-flex; align-items:center; gap:6px; margin-top:24px; font-size:0.82rem; font-weight:600; text-decoration:none; transition:gap 0.2s,color 0.35s; }
  .srv-link.lime { color:var(--lime); } .srv-link.blue { color:var(--blue); }
  .srv-link:hover { gap:10px; }

  /* HOW */
  .how-section { background:var(--bg); transition:background 0.35s; }
  .steps-row { display:grid; grid-template-columns:repeat(4,1fr); gap:24px; margin-top:56px; position:relative; }
  .steps-row::before { content:''; position:absolute; top:26px; left:64px; right:64px; height:1px; background:linear-gradient(90deg,transparent,var(--border),var(--lime),var(--blue),var(--border),transparent); }
  .step-card { background:var(--card); border:1px solid var(--border-dim); border-radius:16px; padding:32px 28px; transition:border-color 0.3s,transform 0.3s,background 0.35s; }
  .step-card:nth-child(odd):hover { border-color:var(--lime); transform:translateY(-3px); }
  .step-card:nth-child(even):hover { border-color:var(--blue); transform:translateY(-3px); }
  .step-num-circle { width:52px; height:52px; border-radius:50%; background:var(--bg); border:2px solid var(--border); display:flex; align-items:center; justify-content:center; font-family:'Bricolage Grotesque',sans-serif; font-size:1rem; font-weight:800; margin-bottom:20px; position:relative; z-index:1; transition:border-color 0.3s,background 0.3s,color 0.3s; }
  .step-card:nth-child(odd) .step-num-circle { color:var(--lime); }
  .step-card:nth-child(even) .step-num-circle { color:var(--blue); }
  .step-card:nth-child(odd):hover .step-num-circle { border-color:var(--lime); background:var(--lime-glow); }
  .step-card:nth-child(even):hover .step-num-circle { border-color:var(--blue); background:var(--blue-glow); }
  .step-title { font-family:'Bricolage Grotesque',sans-serif; font-size:0.98rem; font-weight:700; color:var(--fg); margin-bottom:10px; line-height:1.2; transition:color 0.35s; }
  .step-desc { font-size:0.82rem; color:var(--fg2); line-height:1.6; transition:color 0.35s; }

  /* DESTINATIONS */
  .dest-section { background:var(--surface); transition:background 0.35s; }
  .dest-scroll { display:grid; grid-template-columns:repeat(4,1fr); gap:16px; margin-top:56px; }
  .dest-card { background:var(--card); border:1px solid var(--border-dim); border-radius:16px; overflow:hidden; position:relative; cursor:pointer; aspect-ratio:3/4; display:flex; flex-direction:column; justify-content:flex-end; transition:transform 0.3s,border-color 0.3s; }
  .dest-card:nth-child(odd):hover { border-color:var(--lime); transform:scale(1.025); }
  .dest-card:nth-child(even):hover { border-color:var(--blue); transform:scale(1.025); }
  .dest-emoji-bg { position:absolute; inset:0; display:flex; align-items:center; justify-content:center; font-size:6rem; opacity:0.08; }
  .dest-gradient { position:absolute; inset:0; background:linear-gradient(to top,color-mix(in srgb,var(--bg) 95%,transparent) 0%,color-mix(in srgb,var(--bg) 30%,transparent) 60%,transparent 100%); transition:background 0.35s; }
  .dest-content { position:relative; z-index:1; padding:24px 20px; }
  .dest-flag { font-size:1.8rem; margin-bottom:8px; }
  .dest-name { font-family:'Bricolage Grotesque',sans-serif; font-size:1.25rem; font-weight:800; color:var(--fg); line-height:1; margin-bottom:4px; transition:color 0.35s; }
  .dest-meta { font-size:0.77rem; color:var(--fg2); transition:color 0.35s; }
  .dest-tag { position:absolute; top:16px; left:16px; padding:3px 10px; border-radius:100px; font-size:0.68rem; font-weight:700; letter-spacing:0.06em; text-transform:uppercase; }
  .dest-tag.lime { background:var(--lime-glow); border:1px solid rgba(200,241,53,0.25); color:var(--lime); }
  .dest-tag.blue { background:var(--blue-glow); border:1px solid rgba(75,123,245,0.25); color:var(--blue); }

  /* STATS */
  .stats-wrapper { padding:0 48px 100px; }
  .stats-inner { border-radius:20px; overflow:hidden; display:grid; grid-template-columns:repeat(3,1fr); border:1px solid var(--border-dim); background:var(--card); transition:background 0.35s,border-color 0.35s; }
  .stat-block { padding:56px 48px; border-right:1px solid var(--border-dim); position:relative; overflow:hidden; transition:border-color 0.35s; }
  .stat-block:last-child { border-right:none; }
  .stat-block::before { content:''; position:absolute; bottom:0; left:0; right:0; height:3px; transform:scaleX(0); transform-origin:left; transition:transform 0.5s; }
  .stat-block:nth-child(1)::before { background:var(--lime); }
  .stat-block:nth-child(2)::before { background:var(--blue); }
  .stat-block:nth-child(3)::before { background:var(--lime); }
  .stat-block:hover::before { transform:scaleX(1); }
  .stat-num { font-family:'Bricolage Grotesque',sans-serif; font-size:3.8rem; font-weight:800; line-height:1; letter-spacing:-0.03em; color:var(--fg); margin-bottom:6px; transition:color 0.35s; }
  .suf-lime { color:var(--lime); } .suf-blue { color:var(--blue); }
  .stat-label { font-size:0.87rem; color:var(--fg2); transition:color 0.35s; }

  /* TESTIMONIALS */
  .testimonials-section { background:var(--bg); transition:background 0.35s; }
  .testimonials-grid { display:grid; grid-template-columns:repeat(3,1fr); gap:20px; margin-top:56px; }
  .testi-card { background:var(--card); border:1px solid var(--border-dim); border-radius:16px; padding:32px; transition:border-color 0.3s,transform 0.3s,background 0.35s; }
  .testi-card:nth-child(odd):hover { border-color:rgba(200,241,53,0.35); transform:translateY(-3px); }
  .testi-card:nth-child(even):hover { border-color:rgba(75,123,245,0.35); transform:translateY(-3px); }
  .testi-stars { font-size:0.85rem; letter-spacing:2px; margin-bottom:18px; }
  .testi-stars.lime { color:var(--lime); } .testi-stars.blue { color:var(--blue); }
  .testi-text { font-size:0.89rem; color:var(--fg2); line-height:1.7; margin-bottom:24px; font-style:italic; transition:color 0.35s; }
  .testi-author { display:flex; align-items:center; gap:12px; }
  .testi-avatar { width:42px; height:42px; border-radius:50%; display:flex; align-items:center; justify-content:center; font-size:1rem; }
  .testi-avatar.lime { background:var(--lime-glow); border:1px solid rgba(200,241,53,0.2); }
  .testi-avatar.blue { background:var(--blue-glow); border:1px solid rgba(75,123,245,0.2); }
  .testi-name { font-size:0.87rem; font-weight:600; color:var(--fg); transition:color 0.35s; }
  .testi-role { font-size:0.75rem; color:var(--fg3); transition:color 0.35s; }

  /* PRICING */
  .pricing-section { background:var(--surface); transition:background 0.35s; }
  .pricing-grid { display:grid; grid-template-columns:repeat(3,1fr); gap:20px; margin-top:56px; }
  .price-card { background:var(--card); border:1px solid var(--border-dim); border-radius:16px; padding:36px; position:relative; transition:border-color 0.3s,background 0.35s; }
  .price-card:hover { border-color:var(--blue); }
  .price-card.featured { background:var(--bg); border-color:var(--lime); box-shadow:0 0 40px var(--lime-glow); }
  .price-card.featured:hover { border-color:var(--lime); }
  .price-badge { position:absolute; top:-12px; left:50%; transform:translateX(-50%); background:var(--lime); color:#0D0E0F; padding:3px 14px; border-radius:100px; font-size:0.72rem; font-weight:800; letter-spacing:0.06em; text-transform:uppercase; white-space:nowrap; }
  .price-name { font-family:'Bricolage Grotesque',sans-serif; font-size:0.88rem; font-weight:700; color:var(--fg2); text-transform:uppercase; letter-spacing:0.06em; margin-bottom:12px; transition:color 0.35s; }
  .price-amount { font-family:'Bricolage Grotesque',sans-serif; font-size:3rem; font-weight:800; color:var(--fg); letter-spacing:-0.03em; line-height:1; margin-bottom:6px; transition:color 0.35s; }
  .price-period { font-size:0.8rem; color:var(--fg3); margin-bottom:28px; transition:color 0.35s; }
  .price-divider { height:1px; background:var(--border-dim); margin-bottom:24px; transition:background 0.35s; }
  .price-feature { display:flex; align-items:flex-start; gap:10px; font-size:0.85rem; color:var(--fg2); margin-bottom:12px; line-height:1.4; transition:color 0.35s; }
  .pck-l { color:var(--lime); font-size:0.8rem; flex-shrink:0; margin-top:2px; }
  .pck-b { color:var(--blue); font-size:0.8rem; flex-shrink:0; margin-top:2px; }
  .price-cta { display:block; text-align:center; margin-top:28px; padding:12px; border-radius:10px; text-decoration:none; font-size:0.88rem; font-weight:700; transition:all 0.25s; }
  .price-cta.lime-cta { background:var(--lime); color:#0D0E0F; }
  .price-cta.lime-cta:hover { background:var(--lime-dim); box-shadow:0 0 20px var(--lime-glow); }
  .price-cta.blue-cta { background:none; border:1px solid var(--border); color:var(--fg); }
  .price-cta.blue-cta:hover { border-color:var(--blue); color:var(--blue); }

  /* CTA */
  .cta-section { padding:120px 48px; background:var(--bg); position:relative; overflow:hidden; text-align:center; transition:background 0.35s; }
  .cta-glow-lime { position:absolute; top:50%; left:35%; transform:translate(-50%,-50%); width:500px; height:400px; border-radius:50%; background:radial-gradient(ellipse,var(--lime-glow) 0%,transparent 70%); pointer-events:none; }
  .cta-glow-blue { position:absolute; top:50%; right:10%; transform:translateY(-50%); width:400px; height:400px; border-radius:50%; background:radial-gradient(ellipse,var(--blue-glow) 0%,transparent 70%); pointer-events:none; }
  .cta-inner { position:relative; max-width:680px; margin:0 auto; }
  .cta-inner .section-p { margin:0 auto 40px; text-align:center; max-width:480px; }
  .cta-buttons { display:flex; gap:12px; justify-content:center; flex-wrap:wrap; }

  /* FOOTER */
  footer { background:var(--surface); border-top:1px solid var(--border-dim); padding:72px 48px 32px; transition:background 0.35s,border-color 0.35s; }
  .footer-grid { display:grid; grid-template-columns:1.6fr repeat(3,1fr); gap:48px; margin-bottom:56px; }
  .footer-brand-desc { font-size:0.85rem; color:var(--fg2); line-height:1.6; margin-top:16px; max-width:260px; transition:color 0.35s; }
  .footer-col-title { font-size:0.72rem; font-weight:700; letter-spacing:0.1em; text-transform:uppercase; color:var(--fg3); margin-bottom:18px; transition:color 0.35s; }
  .footer-links { list-style:none; }
  .footer-links li { margin-bottom:10px; }
  .footer-links a { text-decoration:none; color:var(--fg2); font-size:0.86rem; transition:color 0.2s; }
  .footer-links a:hover { color:var(--lime); }
  .footer-bottom { padding-top:24px; border-top:1px solid var(--border-dim); display:flex; justify-content:space-between; align-items:center; font-size:0.77rem; color:var(--fg3); transition:border-color 0.35s,color 0.35s; }
  .footer-accent { color:var(--blue); }

  /* RESPONSIVE */
  @media(max-width:1100px){
    .nav-center a { padding:6px 9px; font-size:0.8rem; }
  }
  @media(max-width:900px){
    nav { padding:12px 20px; }
    .nav-center { display:none; }
    .hero { padding:110px 24px 60px; }
    .hero h1 { font-size:2.6rem; }
    .section { padding:70px 24px; }
    .services-header { grid-template-columns:1fr; gap:24px; }
    .services-grid { grid-template-columns:1fr; }
    .steps-row { grid-template-columns:1fr 1fr; }
    .steps-row::before { display:none; }
    .dest-scroll { grid-template-columns:1fr 1fr; }
    .stats-wrapper { padding:0 24px 70px; }
    .stats-inner { grid-template-columns:1fr; }
    .stat-block { border-right:none; border-bottom:1px solid var(--border-dim); }
    .stat-block:last-child { border-bottom:none; }
    .testimonials-grid { grid-template-columns:1fr; }
    .pricing-grid { grid-template-columns:1fr; }
    .cta-section { padding:80px 24px; }
    footer { padding:56px 24px 24px; }
    .footer-grid { grid-template-columns:1fr; gap:32px; }
    .footer-bottom { flex-direction:column; gap:8px; text-align:center; }
    .trust-bar { padding:20px 24px; gap:20px; }
  }
`;

function applyTheme(tokens) {
  const r = document.documentElement;
  Object.entries(tokens).forEach(([k,v]) => r.style.setProperty(k,v));
}

const SERVICES_DATA = [
  { icon:"🛂", accent:"lime" },
  { icon:"🏠", accent:"blue" },
  { icon:"🌐", accent:"lime" },
];
const STEPS_NUMS = ["01","02","03","04"];
const DEST_NAMES = ["Madrid","Barcelona","Sevilla","Valencia"];
const DEST_EMOJIS = ["🏛️","🌊","🌺","🍊"];
const DEST_TAG_COLORS = ["lime","blue","blue","lime"];
const TESTI_NAMES = ["Ana García","James O'Brien","Yuki Tanaka"];
const TESTI_AV = ["👩‍💼","👨‍💻","👩‍🎨"];
const PLAN_PRICES = ["1.200€","2.200€","3.000€"];

export default function YouNomad() {
  const [dark, setDark] = useState(true);
  const [lang, setLang] = useState("es");
  const [langOpen, setLangOpen] = useState(false);
  const [scrolled, setScrolled] = useState(false);
  const langRef = useRef(null);

  const t = T[lang];
  const currentLang = LANGS.find(l => l.code === lang);

  useEffect(() => { applyTheme(dark ? DARK : LIGHT); }, [dark]);

  useEffect(() => {
    const fn = () => setScrolled(window.scrollY > 50);
    window.addEventListener("scroll", fn);
    return () => window.removeEventListener("scroll", fn);
  }, []);

  useEffect(() => {
    const fn = (e) => { if (langRef.current && !langRef.current.contains(e.target)) setLangOpen(false); };
    document.addEventListener("mousedown", fn);
    return () => document.removeEventListener("mousedown", fn);
  }, []);

  const selectLang = (code) => { setLang(code); setLangOpen(false); };

  return (
    <>
      <style>{css}</style>

      {/* ── NAV ── */}
      <nav className={scrolled ? "compact" : ""}>
        <a href="#" className="logo">
          <div className="logo-mark"><div className="logo-mark-inner"/></div>
          YouNomad
        </a>
        <div className="nav-center">
          <a href="#">{t.navPersons}</a>
          <a href="#">{t.navCompanies}</a>
          <a href="#">{t.navDestinations}</a>
          <a href="#">{t.navPricing}</a>
        </div>
        <div className="nav-right">
          {/* Language Picker */}
          <div className="lang-picker" ref={langRef}>
            <button
              className={`lang-btn${langOpen ? " open" : ""}`}
              onClick={() => setLangOpen(o => !o)}
              aria-label="Select language"
            >
              <span>{currentLang.flag}</span>
              <span>{currentLang.code.toUpperCase()}</span>
              <span className="chevron">▼</span>
            </button>
            {langOpen && (
              <div className="lang-dropdown">
                {LANGS.map(l => (
                  <div
                    key={l.code}
                    className={`lang-option${lang === l.code ? " active" : ""}`}
                    onClick={() => selectLang(l.code)}
                  >
                    <span className="lang-flag">{l.flag}</span>
                    <span className="lang-name">{l.label}</span>
                    {lang === l.code && <span className="lang-check">✓</span>}
                  </div>
                ))}
              </div>
            )}
          </div>
          <button className="mode-toggle" onClick={() => setDark(d=>!d)} title={dark?"Light mode":"Dark mode"}>
            {dark ? "☀️" : "🌙"}
          </button>
          <a href="#" className="btn-lime">{t.navCta}</a>
        </div>
      </nav>

      {/* ── HERO ── */}
      <section className="hero">
        <div className="hero-glow-lime"/><div className="hero-glow-blue"/><div className="hero-grid"/>
        <div className="hero-badge"><span className="badge-dot"/>{t.heroBadge}</div>
        <h1>
          {t.heroH1a}<br/>
          <span className="ab">{t.heroH1b}</span> {t.heroH1c} <span className="al">{t.heroH1d}</span><br/>
          {t.heroH1e}
        </h1>
        <p className="hero-sub">{t.heroSub}</p>
        <div className="hero-actions">
          <a href="#" className="btn-lime-lg">{t.heroCta1}</a>
          <a href="#" className="btn-blue-lg">{t.heroCta2}</a>
        </div>
        <div className="hero-pills">
          {[t.heroPill1, t.heroPill2, t.heroPill3].map(p => (
            <span className="hero-pill" key={p}><span className="pill-x">✕</span>{p}</span>
          ))}
        </div>
      </section>

      {/* ── TRUST ── */}
      <div className="trust-bar">
        <span className="trust-label">{t.trustLabel}</span>
        <div className="trust-logos">
          {["Deloitte Legal","Bankinter","Idealista","Sanitas","WeWork","N26"].map(n=>(
            <span className="trust-logo" key={n}>{n}</span>
          ))}
        </div>
      </div>

      {/* ── SERVICES ── */}
      <section className="section services-section">
        <div className="services-header">
          <div>
            <div className="section-label lime">{t.srvLabel}</div>
            <h2 className="section-h2">{t.srvH2a}<br/>{t.srvH2b}<br/><span className="al">{t.srvH2c}</span></h2>
          </div>
          <div><p className="section-p">{t.srvP}</p></div>
        </div>
        <div className="services-grid">
          {SERVICES_DATA.map((s,i) => {
            const titles = [t.srv1Title, t.srv2Title, t.srv3Title];
            const descs  = [t.srv1Desc,  t.srv2Desc,  t.srv3Desc];
            return (
              <div className="srv-card" key={i}>
                <div className="srv-num">{String(i+1).padStart(2,"0")}</div>
                <div className={`srv-icon ${s.accent}-icon`}>{s.icon}</div>
                <div className="srv-title">{titles[i]}</div>
                <div className="srv-desc">{descs[i]}</div>
                <a href="#" className={`srv-link ${s.accent}`}>{t.srvMore}</a>
              </div>
            );
          })}
        </div>
      </section>

      {/* ── HOW ── */}
      <section className="section how-section">
        <div className="section-label blue">{t.howLabel}</div>
        <h2 className="section-h2">{t.howH2a} <span className="ab">{t.howH2b}</span>{t.howH2c ? ` ${t.howH2c}` : ""}</h2>
        <p className="section-p">{t.howP}</p>
        <div className="steps-row">
          {STEPS_NUMS.map((num,i) => {
            const titles = [t.step1Title,t.step2Title,t.step3Title,t.step4Title];
            const descs  = [t.step1Desc, t.step2Desc, t.step3Desc, t.step4Desc];
            return (
              <div className="step-card" key={num}>
                <div className="step-num-circle">{num}</div>
                <div className="step-title">{titles[i]}</div>
                <div className="step-desc">{descs[i]}</div>
              </div>
            );
          })}
        </div>
      </section>

      {/* ── DESTINATIONS ── */}
      <section className="section dest-section">
        <div className="section-label lime">{t.destLabel}</div>
        <h2 className="section-h2">{t.destH2a}<br/><span className="al">{t.destH2b}</span> {t.destH2c} <span className="ab">{t.destH2d}</span></h2>
        <p className="section-p">{t.destP}</p>
        <div className="dest-scroll">
          {DEST_NAMES.map((name,i) => (
            <div className="dest-card" key={name}>
              <div className="dest-emoji-bg">{DEST_EMOJIS[i]}</div>
              <div className="dest-gradient"/>
              <div className={`dest-tag ${DEST_TAG_COLORS[i]}`}>{t.destTags[i]}</div>
              <div className="dest-content">
                <div className="dest-flag">🇪🇸</div>
                <div className="dest-name">{name}</div>
                <div className="dest-meta">{t.destMeta[i]}</div>
              </div>
            </div>
          ))}
        </div>
      </section>

      {/* ── STATS ── */}
      <div className="stats-wrapper">
        <div className="stats-inner">
          {[
            {num:"2,400",suf:"+",sc:"suf-lime",label:t.stat1Label},
            {num:"98",suf:"%",sc:"suf-blue",label:t.stat2Label},
            {num:"4",suf:" 城",sc:"suf-lime",label:t.stat3Label},
          ].map((s,i) => (
            <div className="stat-block" key={i}>
              <div className="stat-num">{s.num}<span className={s.sc}>{i===2?"":s.suf}</span>{i===2 ? <span className="suf-lime"> {lang==="fi"?"":"+"}</span> : null}</div>
              <div className="stat-label">{s.label}</div>
            </div>
          ))}
        </div>
      </div>

      {/* ── TESTIMONIALS ── */}
      <section className="section testimonials-section">
        <div className="section-label lime">{t.testiLabel}</div>
        <h2 className="section-h2">{t.testiH2a} <span className="ab">{t.testiH2b}</span></h2>
        <div className="testimonials-grid">
          {TESTI_NAMES.map((name,i) => (
            <div className="testi-card" key={name}>
              <div className={`testi-stars ${i%2===0?"lime":"blue"}`}>★★★★★</div>
              <p className="testi-text">"{t.testiQuotes[i]}"</p>
              <div className="testi-author">
                <div className={`testi-avatar ${i%2===0?"lime":"blue"}`}>{TESTI_AV[i]}</div>
                <div>
                  <div className="testi-name">{name}</div>
                  <div className="testi-role">{t.testiRoles[i]}</div>
                </div>
              </div>
            </div>
          ))}
        </div>
      </section>

      {/* ── PRICING ── */}
      <section className="section pricing-section">
        <div className="section-label blue">{t.priceLabel}</div>
        <h2 className="section-h2">{t.priceH2a}<br/><span className="al">{t.priceH2b}</span></h2>
        <p className="section-p">{t.priceP}</p>
        <div className="pricing-grid">
          {[0,1,2].map(pi => (
            <div className={`price-card${pi===1?" featured":""}`} key={pi}>
              {pi===1 && <div className="price-badge">{t.planBadge}</div>}
              <div className="price-name">{t.planNames[pi]}</div>
              <div className="price-amount">{PLAN_PRICES[pi]}</div>
              <div className="price-period">{t.planPeriod}</div>
              <div className="price-divider"/>
              {t.planFeatures[pi].map(f => (
                <div className="price-feature" key={f}>
                  <span className={pi%2===0?"pck-l":"pck-b"}>✓</span>
                  <span>{f}</span>
                </div>
              ))}
              <a href="#" className={`price-cta ${pi===1?"lime-cta":"blue-cta"}`}>{t.planCta}</a>
            </div>
          ))}
        </div>
      </section>

      {/* ── CTA ── */}
      <section className="cta-section">
        <div className="cta-glow-lime"/><div className="cta-glow-blue"/>
        <div className="cta-inner">
          <div className="section-label lime">{t.ctaLabel}</div>
          <h2 className="section-h2">{t.ctaH2a}<br/><span className="al">{t.ctaH2b}</span> <span className="ab">{t.ctaH2c}</span></h2>
          <p className="section-p">{t.ctaP}</p>
          <div className="cta-buttons">
            <a href="#" className="btn-lime-lg">{t.ctaCta1}</a>
            <a href="#" className="btn-blue-lg">{t.ctaCta2}</a>
          </div>
        </div>
      </section>

      {/* ── FOOTER ── */}
      <footer>
        <div className="footer-grid">
          <div>
            <div className="logo" style={{marginBottom:0}}>
              <div className="logo-mark"><div className="logo-mark-inner"/></div>
              YouNomad
            </div>
            <p className="footer-brand-desc">{t.footerDesc}</p>
          </div>
          <div>
            <div className="footer-col-title">{t.footerCol1}</div>
            <ul className="footer-links">
              {t.footerLinks1.map(l=><li key={l}><a href="#">{l}</a></li>)}
            </ul>
          </div>
          <div>
            <div className="footer-col-title">{t.footerCol2}</div>
            <ul className="footer-links">
              {t.footerLinks2.map(l=><li key={l}><a href="#">{l}</a></li>)}
            </ul>
          </div>
          <div>
            <div className="footer-col-title">{t.footerCol3}</div>
            <ul className="footer-links">
              {t.footerLinks3.map(l=><li key={l}><a href="#">{l}</a></li>)}
            </ul>
          </div>
        </div>
        <div className="footer-bottom">
          <span>{t.footerCopy}</span>
          <span>🇪🇸 Madrid · <span className="footer-accent">{t.footerTagline}</span></span>
        </div>
      </footer>
    </>
  );
}
