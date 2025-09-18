# sap_dm_pods_overview
# Overview PODS SAP DM
<!DOCTYPE html>
<html lang="nl">
<head>
  <meta charset="UTF-8">
  <title>PODS Overzicht</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      display: flex;
      height: 100vh;
      background-color: #f9f9f9;
    }
    .sidebar {
      width: 250px;
      background-color: #1e3d2f; /* racing green */
      color: white;
      display: flex;
      flex-direction: column;
      padding: 15px;
    }
    .sidebar h2 {
      margin: 0 0 20px 0;
      font-size: 18px;
    }
    .sidebar select {
      margin-bottom: 20px;
      padding: 5px;
      border-radius: 5px;
      border: none;
    }
    .sidebar a {
      display: block;
      padding: 10px;
      margin: 5px 0;
      background-color: #2f5d47;
      color: white;
      text-decoration: none;
      text-align: center;
      border-radius: 8px;
      transition: background-color 0.2s;
    }
    .sidebar a:hover {
      background-color: #3f7d63;
    }
    .content {
      flex: 1;
      padding: 20px;
      overflow-y: auto;
    }
    header {
      padding: 10px;
      color: white;
      font-weight: bold;
      text-align: center;
    }
    header.dev {
      background-color: #2e7d32; /* groen */
    }
    header.live {
      background-color: #c62828; /* rood */
    }
    .accordion-item {
      margin-bottom: 10px;
    }
    .accordion-header {
      background-color: #eee;
      padding: 10px;
      cursor: pointer;
      font-weight: bold;
      border-radius: 5px;
    }
    .accordion-content {
      max-height: 0;
      overflow: hidden;
      transition: max-height 0.3s ease, padding 0.3s ease;
      padding: 0 10px;
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
    }
    .accordion-content.open {
      padding: 10px;
    }
    .tile { 
      width: 220px;
      padding: 15px;
      text-align: center;
      background-color: #2f5d47; /* racing green */
      color: white;
      border-radius: 10px;
      border: none;
      box-shadow: 2px 2px 6px rgba(0,0,0,0.1);
      cursor: pointer;
      transition: transform 0.2s, box-shadow 0.2s, background-color 0.2s;
    }
    .tile:hover {
      background-color: #3f7d63; /* lichter groen */
      transform: scale(1.05);
      box-shadow: 4px 4px 12px rgba(0,0,0,0.2);
    }
  </style>
</head>
<body>
  <div class="sidebar">
    <h2 data-translate="sidebar.title">Navigatie</h2>
    <select id="languageSelect">
      <option value="en">English</option>
      <option value="nl">Nederlands</option>
      <option value="fr">Français</option>
      <option value="pt">Português</option>
    </select>
    <a href="#" onclick="loadEnv('dev')" data-translate="sidebar.dev">Decofib (P040) DEV</a>
    <a href="#" onclick="loadEnv('live')" data-translate="sidebar.live">Decofib (P040) LIVE</a>
  </div>

  <div class="content" id="contentArea">
    <header id="envHeader" class="dev" data-translate="banner.dev">DEV OMGEVING</header>
    <div id="groupsContainer"></div>
  </div>

  <script>
    const translations = {
      nl: {
        "sidebar.title": "Navigatie",
        "sidebar.dev": "Decofib (P040) DEV",
        "sidebar.live": "Decofib (P040) LIVE",
        "banner.dev": "DEV OMGEVING",
        "banner.live": "⚠ LIVE OMGEVING",
        "group.grading": "Grading",
        "group.preparation": "Voorbereiding",
        "group.splice1": "Voeglijn 1",
        "group.splice2": "Voeglijn 2",
        "group.trimming": "Trimming",
        "group.sortrepair": "Sorteren en herstellen",
        "group.sortonly": "Sorteren",
        "group.repaironly": "Herstellen",
        "tile.grading1": "Gradingtafel 1",
        "tile.grading2": "Gradingtafel 2",
        "tile.prep1": "Voorbereidingstafel 1",
        "tile.prep2": "Voorbereidingstafel 2",
        "tile.splice1in": "Splicing lijn 1 input",
        "tile.splice1m2": "Splicingmachine 2",
		"tile.splice1m3": "Splicingmachine 3",
        "tile.splice1m4": "Splicingmachine 4",
        "tile.splice2in": "Splicing lijn 2 input",
        "tile.splice2m5": "Splicingmachine 5",
        "tile.splice2m6": "Splicingmachine 6",
        "tile.splice2m7": "Splicingmachine 7",
        "tile.splice2m8": "Splicingmachine 8",
        "tile.trim1": "Trimminglijn 1",
        "tile.trim2": "Trimminglijn 2",
        "tile.trim3": "Trimminglijn 3",
        "tile.cut1": "Cutline 1",
        "tile.sortrepair1": "Sort & Repair tafel 1",
        "tile.sortrepair2": "Sort & Repair tafel 2",
        "tile.sortrepair3": "Sort & Repair tafel 3",
        "tile.sortonly1": "Sorting tafel 1",
        "tile.sortonly2": "Sorting tafel 2",
        "tile.sortonly3": "Sorting tafel 3",
        "tile.sortonly4": "Sorting tafel 4",
        "tile.sortonly20": "Temp sorting tafel 20",
        "tile.repair1": "Repair tafel 1",
        "tile.repair2": "Repair tafel 2",
        "tile.repair3": "Repair tafel 3",
        "tile.repair4": "Repair tafel 4",
        "tile.repair5": "Repair tafel 5",
        "tile.repair6": "Repair tafel 6"
      },
      en: {
        "sidebar.title": "Navigation",
        "sidebar.dev": "Decofib (P040) DEV",
        "sidebar.live": "Decofib (P040) LIVE",
        "banner.dev": "DEV ENVIRONMENT",
        "banner.live": "⚠ LIVE ENVIRONMENT",
        "group.grading": "Grading",
        "group.preparation": "Preparation",
        "group.splice1": "Splicing line 1",
        "group.splice2": "Splicing line 2",
        "group.trimming": "Trimming",
        "group.sortrepair": "Sort and repair",
        "group.sortonly": "Sort only",
        "group.repaironly": "Repair only",
        "tile.grading1": "Grading table 1",
        "tile.grading2": "Grading table 2",
        "tile.prep1": "Preparation table 1",
        "tile.prep2": "Preparation table 2",
        "tile.splice1in": "Splicing line 1 input",
        "tile.splice1m2": "Splicing machine 2",
        "tile.splice1m3": "Splicing machine 3",
        "tile.splice1m4": "Splicing machine 4",
        "tile.splice2in": "Splicing line 2 input",
        "tile.splice2m5": "Splicing machine 5",
        "tile.splice2m6": "Splicing machine 6",
        "tile.splice2m7": "Splicing machine 7",
        "tile.splice2m8": "Splicing machine 8",
        "tile.trim1": "Trimming line 1",
        "tile.trim2": "Trimming line 2",
        "tile.trim3": "Trimming line 3",
        "tile.cut1": "Cutline 1",
        "tile.sortrepair1": "Sort & Repair table 1",
        "tile.sortrepair2": "Sort & Repair table 2",
        "tile.sortrepair3": "Sort & Repair table 3",
        "tile.sortonly1": "Sorting table 1",
        "tile.sortonly2": "Sorting table 2",
        "tile.sortonly3": "Sorting table 3",
        "tile.sortonly4": "Sorting table 4",
        "tile.sortonly20": "Temp sorting table 20",
        "tile.repair1": "Repair table 1",
        "tile.repair2": "Repair table 2",
        "tile.repair3": "Repair table 3",
        "tile.repair4": "Repair table 4",
        "tile.repair5": "Repair table 5",
        "tile.repair6": "Repair table 6"
      },
      fr: {
        "sidebar.title": "Navigation",
        "sidebar.dev": "Decofib (P040) DEV",
        "sidebar.live": "Decofib (P040) LIVE",
        "banner.dev": "ENVIRONNEMENT DEV",
        "banner.live": "⚠ ENVIRONNEMENT LIVE",
        "group.grading": "Classement",
        "group.preparation": "Préparation",
        "group.splice1": "Ligne de jointage 1",
        "group.splice2": "Ligne de jointage 2",
        "group.trimming": "Découpe",
        "group.sortrepair": "Triage et réparation",
        "group.sortonly": "Triage",
		"group.repaironly": "Réparation",
        "tile.grading1": "Table de classement 1",
        "tile.grading2": "Table de classement 2",
        "tile.prep1": "Table de préparation 1",
        "tile.prep2": "Table de préparation 2",
        "tile.splice1in": "Entrée ligne 1 jointage",
        "tile.splice1m2": "Machine de jointage 2",
        "tile.splice1m3": "Machine de jointage 3",
        "tile.splice1m4": "Machine de jointage 4",
        "tile.splice2in": "Entrée ligne 2 jointage",
        "tile.splice2m5": "Machine de jointage 5",
        "tile.splice2m6": "Machine de jointage 6",
        "tile.splice2m7": "Machine de jointage 7",
        "tile.splice2m8": "Machine de jointage 8",
        "tile.trim1": "Ligne de découpe 1",
        "tile.trim2": "Ligne de découpe 2",
        "tile.trim3": "Ligne de découpe 3",
        "tile.cut1": "Cutline 1",
        "tile.sortrepair1": "Table tri & réparation 1",
        "tile.sortrepair2": "Table tri & réparation 2",
        "tile.sortrepair3": "Table tri & réparation 3",
        "tile.sortonly1": "Table de tri 1",
        "tile.sortonly2": "Table de tri 2",
        "tile.sortonly3": "Table de tri 3",
        "tile.sortonly4": "Table de tri 4",
		"tile.sortonly20": "Temp table de tri 20",
        "tile.repair1": "Table de réparation 1",
        "tile.repair2": "Table de réparation 2",
        "tile.repair3": "Table de réparation 3",
        "tile.repair4": "Table de réparation 4",
        "tile.repair5": "Table de réparation 5",
        "tile.repair6": "Table de réparation 6"
      },
      pt: {
        "sidebar.title": "Navegação",
        "sidebar.dev": "Decofib (P040) DEV",
        "sidebar.live": "Decofib (P040) LIVE",
        "banner.dev": "AMBIENTE DEV",
        "banner.live": "⚠ AMBIENTE LIVE",
        "group.grading": "Classificação",
        "group.preparation": "Preparação",
        "group.splice1": "Linha de emenda 1",
        "group.splice2": "Linha de emenda 2",
        "group.trimming": "Corte",
        "group.sortrepair": "Classificar e reparar",
        "group.sortonly": "Classificar",
        "group.repaironly": "Reparar",
        "tile.grading1": "Mesa de classificação 1",
        "tile.grading2": "Mesa de classificação 2",
        "tile.prep1": "Mesa de preparação 1",
        "tile.prep2": "Mesa de preparação 2",
        "tile.splice1in": "Entrada linha de emenda 1",
        "tile.splice1m2": "Máquina de emenda 2",
        "tile.splice1m3": "Máquina de emenda 3",
        "tile.splice1m4": "Máquina de emenda 4",
        "tile.splice2in": "Entrada linha de emenda 2",
        "tile.splice2m5": "Máquina de emenda 5",
        "tile.splice2m6": "Máquina de emenda 6",
        "tile.splice2m7": "Máquina de emenda 7",
        "tile.splice2m8": "Máquina de emenda 8",
        "tile.trim1": "Linha de corte 1",
        "tile.trim2": "Linha de corte 2",
        "tile.trim3": "Linha de corte 3",
        "tile.cut1": "Cutline 1",
        "tile.sortrepair1": "Mesa de classificação & reparoção 1",
        "tile.sortrepair2": "Mesa de classificação & reparação 2",
        "tile.sortrepair3": "Mesa de classificação & reparação 3",
        "tile.sortonly1": "Mesa de classificação 1",
        "tile.sortonly2": "Mesa de classificação 2",
        "tile.sortonly3": "Mesa de classificação 3",
        "tile.sortonly4": "Mesa de classificação 4",
        "tile.sortonly20": "Temp mesa de classificação 20",
        "tile.repair1": "Mesa de reparação 1",
        "tile.repair2": "Mesa de reparação 2",
        "tile.repair3": "Mesa de reparação 3",
        "tile.repair4": "Mesa de reparação 4",
        "tile.repair5": "Mesa de reparação 5",
        "tile.repair6": "Mesa de reparação 6"
      }
    };
	
	// Links per tile en per omgeving
	const links = {
	  dev: {
			"tile.grading1":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_GRADE&sap-app-origin-hint=&/?=&WORKCENTER=GRADELN1&RESOURCE=GRADE1",
			"tile.grading2":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_GRADE&sap-app-origin-hint=&/?=&WORKCENTER=GRADELN1&RESOURCE=GRADE2",
			"tile.prep1":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_PREP&sap-app-origin-hint=&/?=&WORKCENTER=MIXMALN1&RESOURCE=MIXMALN1",
			"tile.prep2":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_IN&sap-app-origin-hint=&/?=&WORKCENTER=JOINTLN1&RESOURCE=JOINTLN1",
			"tile.splice1in":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_OUT&sap-app-origin-hint=&/?=&WORKCENTER=JOINTLN1&RESOURCE=JOINT2%20(JOINTLN1)",
			"tile.splice1m2":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_OUT&sap-app-origin-hint=&/?=&WORKCENTER=JOINTLN1&RESOURCE=JOINT3%20(JOINTLN1)",
			"tile.splice1m3":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_OUT&sap-app-origin-hint=&/?=&WORKCENTER=JOINTLN1&RESOURCE=JOINT4%20(JOINTLN1)",
			"tile.splice1m4":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_IN&sap-app-origin-hint=&/?=&WORKCENTER=JOINTLN2&RESOURCE=JOINTLN2",
			"tile.splice2in":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_OUT&sap-app-origin-hint=&/?=&WORKCENTER=JOINTLN2&RESOURCE=JOINT5%20(JOINTLN2)",
			"tile.splice2m5":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_OUT&sap-app-origin-hint=&/?=&WORKCENTER=JOINTLN2&RESOURCE=JOINT6%20(JOINTLN2)",
			"tile.splice2m6":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_OUT&sap-app-origin-hint=&/?=&WORKCENTER=JOINTLN2&RESOURCE=JOINT7%20(JOINTLN5)",
			"tile.splice2m7":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_OUT&sap-app-origin-hint=&/?=&WORKCENTER=JOINTLN2&RESOURCE=JOINT8%20(JOINTLN5)",
			"tile.splice2m8":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_TRIM&sap-app-origin-hint=&/?=&WORKCENTER=CUTSILN1&RESOURCE=CUTSILN1",
			"tile.trim1":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_TRIM&sap-app-origin-hint=&/?=&WORKCENTER=CUTSILN2&RESOURCE=CUTSILN2",
			"tile.trim2":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_TRIM&sap-app-origin-hint=&/?=&WORKCENTER=CUTSILN3&RESOURCE=CUTSILN3",
			"tile.trim3":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SORT_REPAIR&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN1&RESOURCE=SORT1",
			"tile.sortrepair1":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SORT_REPAIR&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN1&RESOURCE=SORT2",
			"tile.sortrepair2":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SORT_REPAIR&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN1&RESOURCE=SORT3",
			"tile.sortrepair3":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SORT_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN2&RESOURCE=SORT4",
			"tile.sortonly1":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SORT_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN2&RESOURCE=SORT5",
			"tile.sortonly2":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SORT_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN2&RESOURCE=SORT6",
			"tile.sortonly3":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SORT_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN2&RESOURCE=SORT7",
			"tile.sortonly4":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_REPAIR_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN3&RESOURCE=SORT8",
			"tile.repair1":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_REPAIR_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN3&RESOURCE=SORT9",
			"tile.repair2":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_REPAIR_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN3&RESOURCE=SORT10",
			"tile.repair3":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_REPAIR_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN3&RESOURCE=SORT11",
			"tile.repair4":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_REPAIR_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN3&RESOURCE=SORT12",
			"tile.repair5":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_REPAIR_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN3&RESOURCE=SORT13",
			"tile.repair6":"https://dcsn-sap-dm-dev.execution.eu20-quality.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_REPAIR_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN3&RESOURCE=SORT13"
	  },
	  live: {
			"tile.grading1":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_GRADE&sap-app-origin-hint=&/?=&WORKCENTER=GRADELN1&RESOURCE=GRADE1",
			"tile.grading2":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_GRADE&sap-app-origin-hint=&/?=&WORKCENTER=GRADELN1&RESOURCE=GRADE2",
			"tile.prep1":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_PREP&sap-app-origin-hint=&/?=&WORKCENTER=PREPLN1&RESOURCE=PREP1",
			"tile.prep2":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_PREP&sap-app-origin-hint=&/?=&WORKCENTER=PREPLN1&RESOURCE=PREP2",
			"tile.splice1in":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_IN&sap-app-origin-hint=&/?=&WORKCENTER=SPLICEL1&RESOURCE=SPLICEL1",
			"tile.splice1m2":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_OUT&sap-app-origin-hint=&/?=&WORKCENTER=SPLICEL1&RESOURCE=SPLICE2",
			"tile.splice1m3":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_OUT&sap-app-origin-hint=&/?=&WORKCENTER=SPLICEL1&RESOURCE=SPLICE3",
			"tile.splice1m4":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_OUT&sap-app-origin-hint=&/?=&WORKCENTER=SPLICEL1&RESOURCE=SPLICE4",
			"tile.splice2in":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_IN&sap-app-origin-hint=&/?=&WORKCENTER=SPLICEL2&RESOURCE=SPLICEL2",
			"tile.splice2m5":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_OUT&sap-app-origin-hint=&/?=&WORKCENTER=SPLICEL2&RESOURCE=SPLICE5",
			"tile.splice2m6":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_OUT&sap-app-origin-hint=&/?=&WORKCENTER=SPLICEL2&RESOURCE=SPLICE6",
			"tile.splice2m7":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_OUT&sap-app-origin-hint=&/?=&WORKCENTER=SPLICEL2&RESOURCE=SPLICE7",
			"tile.splice2m8":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SPLICE_OUT&sap-app-origin-hint=&/?=&WORKCENTER=SPLICEL2&RESOURCE=SPLICE8",
			"tile.trim1":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_TRIM&sap-app-origin-hint=&/?=&WORKCENTER=TRIMLN1&RESOURCE=TRIMLN1",
			"tile.trim2":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_TRIM&sap-app-origin-hint=&/?=&WORKCENTER=TRIMLN2&RESOURCE=TRIMLN2",
			"tile.trim3":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_TRIM&sap-app-origin-hint=&/?=&WORKCENTER=TRIMLN3&RESOURCE=TRIMLN3",
			"tile.cut1":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_TRIM&sap-app-origin-hint=&/?=&WORKCENTER=CUTLN1&RESOURCE=CUTLN1",
			"tile.sortrepair1":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SORT_REPAIR&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN1&RESOURCE=SORT1_1",
			"tile.sortrepair2":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SORT_REPAIR&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN1&RESOURCE=SORT1_2",
			"tile.sortrepair3":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SORT_REPAIR&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN1&RESOURCE=SORT1_3",
			"tile.sortonly1":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SORT_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN2&RESOURCE=SORT2_1",
			"tile.sortonly2":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SORT_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN2&RESOURCE=SORT2_2",
			"tile.sortonly3":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SORT_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN2&RESOURCE=SORT2_3",
			"tile.sortonly4":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SORT_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN2&RESOURCE=SORT2_4",
			"tile.sortonly20":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_SORT_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN2&RESOURCE=SORT2_20",
			"tile.repair1":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_REPAIR_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN3&RESOURCE=SORT3_1",
			"tile.repair2":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_REPAIR_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN3&RESOURCE=SORT3_2",
			"tile.repair3":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_REPAIR_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN3&RESOURCE=SORT3_3",
			"tile.repair4":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_REPAIR_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN3&RESOURCE=SORT3_4",
			"tile.repair5":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_REPAIR_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN3&RESOURCE=SORT3_5",
			"tile.repair6":"https://dcsn-sap-dm-prd.execution.eu20-prod.web.dmc.cloud.sap/cp.portal/site#DMEWorkCenterPOD-Display?POD_ID=ZD_GUIDED_REPAIR_ONLY&sap-app-origin-hint=&/?=&WORKCENTER=SORTLN3&RESOURCE=SORT3_6"
	  }
	};

    const groups = [
	  { key:"group.grading", tiles:["tile.grading1","tile.grading2"] },
	  { key:"group.preparation", tiles:["tile.prep1","tile.prep2"] },
	  { key:"group.splice1", tiles:["tile.splice1in","tile.splice1m2","tile.splice1m3","tile.splice1m4"] },
	  { key:"group.splice2", tiles:["tile.splice2in","tile.splice2m5","tile.splice2m6","tile.splice2m7","tile.splice2m8"] },
	  { key:"group.trimming", tiles:["tile.trim1","tile.trim2","tile.trim3","tile.cut1"] },
	  { key:"group.sortrepair", tiles:["tile.sortrepair1","tile.sortrepair2","tile.sortrepair3"] },
	  { key:"group.sortonly", tiles:["tile.sortonly1","tile.sortonly2","tile.sortonly3","tile.sortonly4","tile.sortonly20"] },
	  { key:"group.repaironly", tiles:["tile.repair1","tile.repair2","tile.repair3","tile.repair4","tile.repair5","tile.repair6"] }
	];

    let currentLang = localStorage.getItem("lang") || "en";
    let currentEnv = localStorage.getItem("env") || "dev";
    let savedGroup = localStorage.getItem("group") || null;

    function renderGroups() {
      const container = document.getElementById("groupsContainer");
      container.innerHTML = "";
      groups.forEach(group => {
        const item = document.createElement("div");
        item.classList.add("accordion-item");

        const header = document.createElement("div");
        header.classList.add("accordion-header");
        header.setAttribute("data-translate", group.key);
        header.textContent = translations[currentLang][group.key];

        const content = document.createElement("div");
        content.classList.add("accordion-content");

        group.tiles.forEach(tileKey => {
          if(!links[currentEnv][tileKey]) return;
          const btn = document.createElement("button");
          btn.classList.add("tile");
          btn.setAttribute("data-translate", tileKey);
          btn.textContent = translations[currentLang][tileKey];
          btn.onclick = () => window.open(links[currentEnv][tileKey], "_blank");
          content.appendChild(btn);
        });

        header.addEventListener("click", () => {
          document.querySelectorAll(".accordion-content").forEach(c => {
            if(c !== content) {
              c.style.maxHeight = null;
              c.classList.remove("open");
            }
          });
          if(content.classList.contains("open")) {
            content.style.maxHeight = null;
            content.classList.remove("open");
          } else {
            content.classList.add("open");
            content.style.maxHeight = content.scrollHeight + "px";
          }
          localStorage.setItem("group", group.key);
        });

        item.appendChild(header);
        item.appendChild(content);
        container.appendChild(item);
      });

      if(savedGroup) {
        const header = document.querySelector(`.accordion-header[data-translate="${savedGroup}"]`);
        if(header) {
          const content = header.nextElementSibling;
          content.classList.add("open");
          content.style.maxHeight = content.scrollHeight + "px";
        }
      }
    }

    function translatePage() {
      document.querySelectorAll("[data-translate]").forEach(el=>{
        const key = el.getAttribute("data-translate");
        if(translations[currentLang][key]) el.textContent = translations[currentLang][key];
      });
    }

    function loadEnv(env) {
      currentEnv = env;
      localStorage.setItem("env", env);
      const header = document.getElementById("envHeader");
      header.className = env;
      header.setAttribute("data-translate", env==="dev"?"banner.dev":"banner.live");
      renderGroups();
      translatePage();
    }

    document.getElementById("languageSelect").value = currentLang;
    document.getElementById("languageSelect").addEventListener("change", e => {
      currentLang = e.target.value;
      localStorage.setItem("lang", currentLang);
      renderGroups();
      translatePage();
    });

    renderGroups();
    translatePage();
    loadEnv(currentEnv);
  </script>
</body>
</html>
