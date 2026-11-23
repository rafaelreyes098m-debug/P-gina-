<?php
session_start();

// Cambia esta clave por la tuya secreta para administrar los enlaces
$claveAdmin = 'admin123';

// Si tienes otro dominio o carpeta sin "panel" que apunte a este mismo sitio, ponlo aquí.
// Déjalo vacío '' para usar el dominio actual.
$baseUrlSinPanel = '';

$uploadBaseDir = __DIR__ . '/uploads/';
$publicBaseDir = 'uploads/';
$dbFile = $uploadBaseDir . 'links_control.json';
$maxBytes = 25 * 1024 * 1024; // 25 MB por archivo
$blocked = ['phtml', 'php3', 'php4', 'php5', 'php7', 'phar', 'htaccess', 'sh', 'exe', 'bat', 'cmd', 'js'];
$imgExts = ['jpg', 'jpeg', 'png', 'gif', 'webp', 'svg', 'ico', 'avif'];

if (!is_dir($uploadBaseDir)) {
    @mkdir($uploadBaseDir, 0755, true);
}

if (isset($_GET['logout_admin'])) {
    unset($_SESSION['dueno_panel_ok']);
    header('Location: ' . strtok($_SERVER['REQUEST_URI'], '?'));
    exit;
}

$errorAdmin = '';
if ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_POST['login_admin'])) {
    if (hash_equals($claveAdmin, (string)($_POST['clave_admin_input'] ?? ''))) {
        $_SESSION['dueno_panel_ok'] = true;
        header('Location: ' . $_SERVER['REQUEST_URI']);
        exit;
    } else {
        $errorAdmin = 'Clave de administrador incorrecta.';
    }
}

$esDueño = !empty($_SESSION['dueno_panel_ok']);

function cargarLinks($dbFile) {
    if (!is_file($dbFile)) {
        return [];
    }
    $raw = @file_get_contents($dbFile);
    $data = json_decode((string)$raw, true);
    return is_array($data) ? $data : [];
}

function guardarLinks($dbFile, array $data) {
    @file_put_contents($dbFile, json_encode($data, JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE), LOCK_EX);
}

function borrarCarpetaRecursiva($dir) {
    if (!is_dir($dir)) {
        return;
    }
    $items = scandir($dir);
    if (!is_array($items)) {
        return;
    }
    foreach ($items as $item) {
        if ($item === '.' || $item === '..') {
            continue;
        }
        $path = $dir . DIRECTORY_SEPARATOR . $item;
        if (is_dir($path)) {
            borrarCarpetaRecursiva($path);
        } else {
            @unlink($path);
        }
    }
    @rmdir($dir);
}

function crearSlugNombre($texto) {
    $texto = trim((string)$texto);
    if ($texto === '') {
        return 'tubanco-tu-futuro';
    }
    $mapa = [
        'á'=>'a','é'=>'e','í'=>'i','ó'=>'o','ú'=>'u','ü'=>'u','ñ'=>'n',
        'Á'=>'a','É'=>'e','Í'=>'i','Ó'=>'o','Ú'=>'u','Ü'=>'u','Ñ'=>'n'
    ];
    $texto = strtr($texto, $mapa);
    $texto = strtolower($texto);
    $texto = preg_replace('/[^a-z0-9]+/', '-', $texto);
    $texto = trim($texto, '-');
    if ($texto === '') {
        return 'tubanco-tu-futuro';
    }
    return substr($texto, 0, 50);
}

function obtenerBaseLimpia($baseUrlSinPanel) {
    $baseUrlSinPanel = trim((string)$baseUrlSinPanel);
    if ($baseUrlSinPanel !== '') {
        return rtrim($baseUrlSinPanel, '/');
    }

    $protocolo = (!empty($_SERVER['HTTPS']) && $_SERVER['HTTPS'] !== 'off') ? 'https' : 'http';
    $host = $_SERVER['HTTP_HOST'] ?? 'localhost';
    $carpetaActual = rtrim(dirname($_SERVER['SCRIPT_NAME'] ?? '/'), '/\\');
    $carpetaActual = ($carpetaActual === '' || $carpetaActual === '.') ? '' : $carpetaActual;

    if ($carpetaActual !== '') {
        $partes = array_values(array_filter(explode('/', trim($carpetaActual, '/')), function ($seg) {
            return stripos($seg, 'panel') === false;
        }));
        $carpetaActual = !empty($partes) ? '/' . implode('/', $partes) : '';
    }

    return $protocolo . '://' . $host . $carpetaActual;
}

$linksData = cargarLinks($dbFile);
$linkReal = '';
$archivoPrincipal = '';
$archivosSubidos = [];
$errores = [];
$mensajeExito = '';
$nombreLinkInput = $_POST['nombre_link'] ?? 'tubanco tu futuro';
$claveLinkInput = $_POST['clave_link'] ?? '';
$activarClaveInput = isset($_POST['activar_clave']) ? true : false;

if ($esDueño && $_SERVER['REQUEST_METHOD'] === 'POST') {
    if (isset($_POST['accion_link'], $_POST['folder_id'])) {
        $fid = basename((string)$_POST['folder_id']);
        $accion = (string)$_POST['accion_link'];

        if (isset($linksData[$fid])) {
            if ($accion === 'guardar_clave') {
                $nuevaClave = trim((string)($_POST['nueva_clave'] ?? ''));
                if ($nuevaClave === '') {
                    $linksData[$fid]['clave_activa'] = false;
                    $linksData[$fid]['clave_hash'] = '';
                    $linksData[$fid]['clave_visible'] = '';
                    guardarLinks($dbFile, $linksData);
                    $mensajeExito = 'Clave quitada del enlace "' . $fid . '". Ahora entra directo.';
                } else {
                    $linksData[$fid]['clave_activa'] = true;
                    $linksData[$fid]['clave_hash'] = password_hash($nuevaClave, PASSWORD_DEFAULT);
                    $linksData[$fid]['clave_visible'] = $nuevaClave;
                    guardarLinks($dbFile, $linksData);
                    $mensajeExito = 'Clave actualizada para "' . $fid . '" (' . $nuevaClave . ').';
                }
            } elseif ($accion === 'quitar_clave') {
                $linksData[$fid]['clave_activa'] = false;
                $linksData[$fid]['clave_hash'] = '';
                $linksData[$fid]['clave_visible'] = '';
                guardarLinks($dbFile, $linksData);
                $mensajeExito = 'Clave quitada del enlace "' . $fid . '".';
            } elseif ($accion === 'toggle_bloqueo') {
                $linksData[$fid]['bloqueado'] = empty($linksData[$fid]['bloqueado']);
                guardarLinks($dbFile, $linksData);
                $mensajeExito = !empty($linksData[$fid]['bloqueado'])
                    ? 'Enlace "' . $fid . '" bloqueado.'
                    : 'Enlace "' . $fid . '" desbloqueado.';
            } elseif ($accion === 'borrar_link') {
                borrarCarpetaRecursiva($uploadBaseDir . $fid);
                unset($linksData[$fid]);
                guardarLinks($dbFile, $linksData);
                $mensajeExito = 'Enlace "' . $fid . '" eliminado.';
            }
        }
    } elseif (isset($_POST['crear_link'])) {
        $claveIngresada = trim((string)($_POST['clave_link'] ?? ''));
        $ponerClave = !empty($_POST['activar_clave']) && $claveIngresada !== '';

        if (!isset($_FILES['archivos']) || empty($_FILES['archivos']['name'][0])) {
            $errores[] = 'Selecciona tu archivo HTML y tu logo.';
        } else {
            $slugBase = crearSlugNombre($nombreLinkInput);
            $packFolder = $slugBase . '-' . substr(bin2hex(random_bytes(3)), 0, 6);
            $targetFolder = $uploadBaseDir . $packFolder . '/';

            if (!is_dir($targetFolder) && !@mkdir($targetFolder, 0755, true)) {
                $errores[] = 'No se pudo crear la carpeta en uploads/. Revisa permisos 755/775.';
            } else {
                $total = count($_FILES['archivos']['name']);
                $baseUrl = obtenerBaseLimpia($baseUrlSinPanel);

                $htmlPaths = [];
                $imagenEncontrada = '';

                for ($i = 0; $i < $total; $i++) {
                    $errCode = $_FILES['archivos']['error'][$i];
                    $origName = basename($_FILES['archivos']['name'][$i]);
                    $size = $_FILES['archivos']['size'][$i];
                    $tmpName = $_FILES['archivos']['tmp_name'][$i];

                    if ($errCode === UPLOAD_ERR_NO_FILE) {
                        continue;
                    }
                    if ($errCode !== UPLOAD_ERR_OK) {
                        $errores[] = $origName . ': error al subir (código ' . (int)$errCode . ').';
                        continue;
                    }
                    if ($size <= 0 || $size > $maxBytes) {
                        $errores[] = $origName . ': está vacío o supera 25 MB.';
                        continue;
                    }

                    $ext = strtolower(pathinfo($origName, PATHINFO_EXTENSION));
                    if ($ext !== '' && in_array($ext, $blocked, true)) {
                        $errores[] = $origName . ': extensión .' . $ext . ' no permitida.';
                        continue;
                    }

                    $base = preg_replace('/[^a-zA-Z0-9._-]/', '_', pathinfo($origName, PATHINFO_FILENAME));
                    $base = trim($base, '._');
                    if ($base === '') {
                        $base = 'archivo';
                    }

                    $nombreLimpio = $base . ($ext !== '' ? '.' . $ext : '');
                    $nombreGuardado = (strtolower($nombreLimpio) === 'index.php') ? 'pagina_subida.php' : $nombreLimpio;
                    $destino = $targetFolder . $nombreGuardado;

                    if (move_uploaded_file($tmpName, $destino)) {
                        $archivosSubidos[] = [
                            'nombre' => $nombreGuardado,
                            'ext' => $ext
                        ];

                        if (in_array($ext, ['html', 'htm', 'php'], true)) {
                            $htmlPaths[] = $nombreGuardado;
                        }
                        if (in_array($ext, $imgExts, true) && $imagenEncontrada === '') {
                            $imagenEncontrada = $nombreGuardado;
                        }
                    } else {
                        $errores[] = $origName . ': no se pudo guardar.';
                    }
                }

                if (!empty($htmlPaths) && $imagenEncontrada !== '') {
                    foreach ($htmlPaths as $hName) {
                        $hFile = $targetFolder . $hName;
                        $htmlContent = @file_get_contents($hFile);
                        if ($htmlContent !== false) {
                            $htmlContent = preg_replace(
                                '/src=(["\'])(?:\.\/|\/|images\/|img\/|assets\/)?[^"\']+\.(?:jpg|jpeg|png|gif|webp|svg|ico|avif)\1/i',
                                'src=$1' . $imagenEncontrada . '$1',
                                $htmlContent
                            );
                            @file_put_contents($hFile, $htmlContent);
                        }
                    }
                }

                $elegido = null;
                foreach ($archivosSubidos as $item) {
                    if (strtolower($item['nombre']) === 'index.html') {
                        $elegido = $item;
                        break;
                    }
                }
                if (!$elegido) {
                    foreach ($archivosSubidos as $item) {
                        if (in_array($item['ext'], ['html', 'htm', 'php'], true)) {
                            $elegido = $item;
                            break;
                        }
                    }
                }
                if (!$elegido && !empty($archivosSubidos)) {
                    $elegido = $archivosSubidos[0];
                }

                if ($elegido) {
                    $targetHtmlExport = var_export($elegido['nombre'], true);
                    $folderIdExport = var_export($packFolder, true);
                    $panelRootExport = var_export(rtrim(str_replace('\\', '/', __DIR__), '/'), true);

                    $folderIndexPhp = "<?php\n"
                        . "session_start();\n"
                        . "\$fid = " . $folderIdExport . ";\n"
                        . "\$htmlFile = __DIR__ . '/' . " . $targetHtmlExport . ";\n"
                        . "\$dbPath = " . $panelRootExport . " . '/uploads/links_control.json';\n"
                        . "\$cfg = ['clave_activa' => false, 'clave_hash' => '', 'bloqueado' => false];\n"
                        . "if (is_file(\$dbPath)) {\n"
                        . "    \$all = json_decode((string)@file_get_contents(\$dbPath), true);\n"
                        . "    if (is_array(\$all) && isset(\$all[\$fid]) && is_array(\$all[\$fid])) {\n"
                        . "        \$cfg = array_merge(\$cfg, \$all[\$fid]);\n"
                        . "    }\n"
                        . "}\n"
                        . "if (!empty(\$cfg['bloqueado'])) {\n"
                        . "    http_response_code(403);\n"
                        . "    echo '<!DOCTYPE html><html lang=\"es\"><head><meta charset=\"UTF-8\"><meta name=\"viewport\" content=\"width=device-width,initial-scale=1.0\"><title>Enlace Pausado</title><style>*{box-sizing:border-box;font-family:Segoe UI,Arial,sans-serif}body{margin:0;min-height:100vh;display:flex;align-items:center;justify-content:center;background:#07090f;color:#f8fafc;padding:18px}.box{max-width:390px;width:100%;background:rgba(16,20,32,.92);border:1px solid rgba(248,113,113,.45);border-radius:18px;padding:24px;text-align:center}h2{margin:0 0 8px;color:#fca5a5}p{margin:0;color:#cbd5e1;font-size:14px}</style></head><body><div class=\"box\"><h2>Enlace No Disponible</h2><p>Este enlace fue pausado por el administrador.</p></div></body></html>';\n"
                        . "    exit;\n"
                        . "}\n"
                        . "\$authSessionKey = 'vip_auth_' . \$fid . '_' . md5((string)\$cfg['clave_hash']);\n"
                        . "\$err = '';\n"
                        . "if (!empty(\$cfg['clave_activa']) && !empty(\$cfg['clave_hash'])) {\n"
                        . "    if (\$_SERVER['REQUEST_METHOD'] === 'POST' && isset(\$_POST['clave_acceso'])) {\n"
                        . "        if (password_verify((string)\$_POST['clave_acceso'], (string)\$cfg['clave_hash'])) {\n"
                        . "            \$_SESSION[\$authSessionKey] = true;\n"
                        . "            header('Location: ' . \$_SERVER['REQUEST_URI']);\n"
                        . "            exit;\n"
                        . "        } else {\n"
                        . "            \$err = 'Clave incorrecta.';\n"
                        . "        }\n"
                        . "    }\n"
                        . "    if (empty(\$_SESSION[\$authSessionKey])) {\n"
                        . "        echo '<!DOCTYPE html><html lang=\"es\"><head><meta charset=\"UTF-8\"><meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\"><title>Acceso con Clave</title><style>*{box-sizing:border-box;font-family:Segoe UI,Arial,sans-serif}body{margin:0;min-height:100vh;display:flex;align-items:center;justify-content:center;background:radial-gradient(circle at 20% 20%,rgba(245,200,76,.18),transparent 45%),#07090f;color:#f8fafc;padding:18px}.card{width:100%;max-width:400px;background:rgba(16,20,32,.92);border:1px solid rgba(245,200,76,.45);border-radius:18px;padding:24px;box-shadow:0 20px 50px rgba(0,0,0,.7),0 0 25px rgba(245,200,76,.15)}h2{margin:0 0 8px;font-size:20px;color:#fff3b0}p{margin:0 0 16px;font-size:13px;color:#cbd5e1}input{width:100%;padding:12px;border-radius:10px;border:1px solid rgba(245,200,76,.35);background:#070a12;color:#fff;font-size:15px;margin-bottom:14px}.btn{width:100%;padding:12px;border:0;border-radius:10px;background:linear-gradient(135deg,#fff3b0,#f5c84c,#b88218);color:#120e04;font-weight:800;font-size:15px;cursor:pointer}.err{margin-top:12px;padding:10px;border-radius:10px;background:rgba(127,29,29,.75);border:1px solid rgba(248,113,113,.45);color:#fecaca;font-size:13px}</style></head><body><div class=\"card\"><h2>✦ Acceso Protegido ✦</h2><p>Escribe la clave asignada para entrar.</p><form method=\"post\"><input type=\"password\" name=\"clave_acceso\" placeholder=\"Clave de acceso\" required autofocus><button class=\"btn\" type=\"submit\">Entrar</button></form>';\n"
                        . "        if (\$err !== '') { echo '<div class=\"err\">' . htmlspecialchars(\$err, ENT_QUOTES, 'UTF-8') . '</div>'; }\n"
                        . "        echo '</div></body></html>';\n"
                        . "        exit;\n"
                        . "    }\n"
                        . "}\n"
                        . "if (is_file(\$htmlFile)) {\n"
                        . "    if (strtolower(pathinfo(\$htmlFile, PATHINFO_EXTENSION)) === 'php') {\n"
                        . "        include \$htmlFile;\n"
                        . "    } else {\n"
                        . "        header('Content-Type: text/html; charset=utf-8');\n"
                        . "        readfile(\$htmlFile);\n"
                        . "    }\n"
                        . "    exit;\n"
                        . "}\n"
                        . "http_response_code(404);\n"
                        . "echo 'Archivo no encontrado';\n";

                    @file_put_contents($targetFolder . 'index.php', $folderIndexPhp);

                    $linkReal = $baseUrl . '/' . $publicBaseDir . rawurlencode($packFolder) . '/';
                    $archivoPrincipal = $elegido['nombre'];

                    $linksData[$packFolder] = [
                        'nombre_base' => $slugBase,
                        'archivo_html' => $elegido['nombre'],
                        'archivos' => array_column($archivosSubidos, 'nombre'),
                        'url' => $linkReal,
                        'clave_activa' => $ponerClave,
                        'clave_hash' => $ponerClave ? password_hash($claveIngresada, PASSWORD_DEFAULT) : '',
                        'clave_visible' => $ponerClave ? $claveIngresada : '',
                        'bloqueado' => false,
                        'creado' => date('Y-m-d H:i')
                    ];
                    guardarLinks($dbFile, $linksData);
                }
            }
        }
    }
}
?>
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>VIP Link Studio — Control Total</title>
  <style>
    :root {
      --gold-1: #fff3b0;
      --gold-2: #f5c84c;
      --gold-3: #b88218;
      --bg-dark: #07090f;
      --card-bg: rgba(16, 20, 32, 0.86);
      --border-glow: rgba(245, 200, 76, 0.35);
      --text-main: #f8fafc;
      --text-muted: #cbd5e1;
    }

    * { box-sizing: border-box; font-family: 'Segoe UI', system-ui, -apple-system, Arial, sans-serif; }

    body {
      margin: 0;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      background:
        radial-gradient(circle at 18% 15%, rgba(245, 200, 76, 0.18), transparent 42%),
        radial-gradient(circle at 85% 80%, rgba(168, 85, 247, 0.16), transparent 45%),
        radial-gradient(circle at 50% 50%, #111625 0%, var(--bg-dark) 100%);
      color: var(--text-main);
      padding: 18px;
      overflow-x: hidden;
    }

    .box {
      position: relative;
      width: 100%;
      max-width: 540px;
      background: var(--card-bg);
      backdrop-filter: blur(18px);
      -webkit-backdrop-filter: blur(18px);
      padding: 24px;
      border-radius: 20px;
      border: 1px solid var(--border-glow);
      box-shadow:
        0 25px 60px rgba(0, 0, 0, 0.65),
        0 0 35px rgba(245, 200, 76, 0.12),
        inset 0 1px 0 rgba(255, 255, 255, 0.16);
      overflow: hidden;
    }

    .top-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 10px;
      margin-bottom: 10px;
    }

    .badge {
      display: inline-block;
      font-size: 11px;
      letter-spacing: 1.4px;
      text-transform: uppercase;
      font-weight: 700;
      padding: 5px 11px;
      border-radius: 999px;
      background: linear-gradient(90deg, rgba(245, 200, 76, 0.2), rgba(255, 243, 176, 0.1));
      border: 1px solid rgba(245, 200, 76, 0.45);
      color: var(--gold-1);
    }

    .logout-link {
      font-size: 12px;
      color: #fca5a5;
      text-decoration: none;
      font-weight: 700;
    }

    h2 {
      margin: 0 0 8px;
      font-size: 22px;
      font-weight: 800;
      background: linear-gradient(135deg, #ffffff 0%, var(--gold-1) 45%, var(--gold-2) 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    .hint {
      margin: 0 0 16px;
      color: var(--text-muted);
      font-size: 13px;
      line-height: 1.45;
    }

    label {
      display: block;
      font-size: 12.5px;
      color: var(--gold-1);
      margin-bottom: 6px;
      font-weight: 700;
    }

    input[type="text"],
    input[type="password"] {
      width: 100%;
      padding: 11px 13px;
      margin-bottom: 14px;
      border-radius: 12px;
      border: 1px solid rgba(245, 200, 76, 0.28);
      background: rgba(7, 10, 18, 0.85);
      color: #fff;
      font-size: 14px;
      outline: none;
    }

    input[type="file"] {
      width: 100%;
      margin-bottom: 16px;
      padding: 11px;
      background: rgba(7, 10, 18, 0.72);
      border: 1px dashed rgba(245, 200, 76, 0.45);
      border-radius: 12px;
      color: #e2e8f0;
      font-size: 13px;
    }

    .check-row {
      display: flex;
      align-items: center;
      gap: 9px;
      margin-bottom: 14px;
      font-size: 13px;
      color: var(--text-main);
    }

    .btn-gold {
      width: 100%;
      padding: 13px 16px;
      border: 0;
      border-radius: 12px;
      background: linear-gradient(135deg, var(--gold-1) 0%, var(--gold-2) 48%, var(--gold-3) 100%);
      color: #120e04;
      font-size: 15px;
      font-weight: 800;
      cursor: pointer;
      box-shadow: 0 8px 24px rgba(245, 200, 76, 0.3);
    }

    .err {
      margin-top: 14px;
      padding: 12px;
      border-radius: 12px;
      background: rgba(127, 29, 29, 0.75);
      border: 1px solid rgba(248, 113, 113, 0.45);
      color: #fecaca;
      font-size: 13px;
    }

    .msg-ok {
      margin-top: 14px;
      padding: 11px 13px;
      border-radius: 12px;
      background: rgba(6, 78, 59, 0.75);
      border: 1px solid rgba(52, 211, 153, 0.45);
      color: #a7f3d0;
      font-size: 13px;
    }

    .ok {
      margin-top: 16px;
      padding: 15px;
      border-radius: 14px;
      background: linear-gradient(160deg, rgba(16, 185, 129, 0.14), rgba(245, 200, 76, 0.08));
      border: 1px solid rgba(245, 200, 76, 0.45);
    }

    .ok input {
      margin: 8px 0 10px;
      font-size: 13px;
    }

    .open-btn {
      display: block;
      text-align: center;
      margin-top: 10px;
      padding: 10px 14px;
      border-radius: 12px;
      background: linear-gradient(135deg, #3b82f6, #1d4ed8);
      color: #fff;
      text-decoration: none;
      font-weight: 700;
      font-size: 13.5px;
    }

    .manage-box {
      margin-top: 22px;
      padding-top: 18px;
      border-top: 1px solid rgba(245, 200, 76, 0.22);
    }

    .manage-title {
      margin: 0 0 12px;
      font-size: 16px;
      color: var(--gold-1);
      font-weight: 800;
    }

    .link-card {
      background: rgba(7, 10, 18, 0.82);
      border: 1px solid rgba(245, 200, 76, 0.25);
      border-radius: 14px;
      padding: 12px;
      margin-bottom: 12px;
    }

    .link-head {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 8px;
      font-size: 13px;
      font-weight: 700;
      color: #fff;
      word-break: break-all;
    }

    .pill {
      font-size: 11px;
      padding: 3px 8px;
      border-radius: 999px;
      font-weight: 700;
      white-space: nowrap;
    }

    .pill-lock { background: rgba(245, 200, 76, 0.22); color: var(--gold-1); border: 1px solid rgba(245, 200, 76, 0.45); }
    .pill-open { background: rgba(16, 185, 129, 0.2); color: #6ee7b7; border: 1px solid rgba(16, 185, 129, 0.4); }
    .pill-ban  { background: rgba(239, 68, 68, 0.22); color: #fca5a5; border: 1px solid rgba(239, 68, 68, 0.45); }

    .link-url {
      font-size: 12px;
      color: #93c5fd;
      margin: 6px 0 10px;
      word-break: break-all;
    }

    .ctrl-form {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
      align-items: center;
    }

    .ctrl-input {
      flex: 1;
      min-width: 130px;
      padding: 8px 10px !important;
      margin: 0 !important;
      font-size: 12.5px !important;
      border-radius: 9px !important;
    }

    .btn-sm {
      padding: 8px 10px;
      border: 0;
      border-radius: 9px;
      font-size: 12px;
      font-weight: 700;
      cursor: pointer;
      color: #fff;
      background: #2563eb;
    }

    .btn-amber { background: #d97706; color: #111827; }
    .btn-gray  { background: #374151; }
    .btn-red   { background: #b91c1c; }
  </style>
</head>
<body>
  <div class="box">
    <?php if (!$esDueño): ?>
      <div class="badge">✦ Panel Privado del Dueño ✦</div>
      <h2>Entrar a tu Panel</h2>
      <p class="hint">Solo tú puedes crear links y poner o quitar claves a las personas.</p>

      <form method="post">
        <label for="clave_admin_input">Tu clave de dueño</label>
        <input id="clave_admin_input" type="password" name="clave_admin_input" placeholder="Escribe tu clave de dueño" required autofocus>
        <button class="btn-gold" type="submit" name="login_admin" value="1">✦ Entrar al panel</button>
      </form>

      <?php if ($errorAdmin !== ''): ?>
        <div class="err"><?= htmlspecialchars($errorAdmin, ENT_QUOTES, 'UTF-8') ?></div>
      <?php endif; ?>

    <?php else: ?>
      <div class="top-bar">
        <div class="badge">✦ Control de Claves VIP ✦</div>
        <a class="logout-link" href="?logout_admin=1">Salir</a>
      </div>
      <h2>Generador y Control de Enlaces</h2>
      <p class="hint">Crea un enlace y decide si lleva clave o si luego se la quitas o bloqueas a alguien.</p>

      <form method="post" enctype="multipart/form-data">
        <label for="nombre_link">Nombre para el link</label>
        <input id="nombre_link" type="text" name="nombre_link" value="<?= htmlspecialchars($nombreLinkInput, ENT_QUOTES, 'UTF-8') ?>" placeholder="Ej: tubanco tu futuro" required>

        <div class="check-row">
          <input id="activar_clave" type="checkbox" name="activar_clave" value="1" <?= $activarClaveInput ? 'checked' : '' ?>>
          <label for="activar_clave" style="margin:0;cursor:pointer;">Activar clave para este enlace</label>
        </div>

        <label for="clave_link">Clave del enlace (opcional si no marcas la casilla)</label>
        <input id="clave_link" type="text" name="clave_link" value="<?= htmlspecialchars($claveLinkInput, ENT_QUOTES, 'UTF-8') ?>" placeholder="Ej: 4829 o nombre">

        <label for="archivos">Archivos (HTML y logo)</label>
        <input id="archivos" type="file" name="archivos[]" multiple required>

        <button class="btn-gold" type="submit" name="crear_link" value="1">✦ Generar link</button>
      </form>

      <?php if (!empty($errores)): ?>
        <div class="err">
          <?php foreach ($errores as $e): ?>
            <div><?= htmlspecialchars($e, ENT_QUOTES, 'UTF-8') ?></div>
          <?php endforeach; ?>
        </div>
      <?php endif; ?>

      <?php if ($mensajeExito !== ''): ?>
        <div class="msg-ok"><?= htmlspecialchars($mensajeExito, ENT_QUOTES, 'UTF-8') ?></div>
      <?php endif; ?>

      <?php if ($linkReal !== ''): ?>
        <div class="ok">
          <div><strong>Link generado (<?= htmlspecialchars($archivoPrincipal, ENT_QUOTES, 'UTF-8') ?>):</strong></div>
          <input type="text" id="enlaceReal" readonly value="<?= htmlspecialchars($linkReal, ENT_QUOTES, 'UTF-8') ?>" onclick="this.select()">
          <button class="btn-gold" id="copyBtn" type="button" onclick="copiarLink()">Copiar link</button>
          <a class="open-btn" href="<?= htmlspecialchars($linkReal, ENT_QUOTES, 'UTF-8') ?>" target="_blank" rel="noopener">Abrir página y ver logo →</a>
        </div>
      <?php endif; ?>

      <div class="manage-box">
        <div class="manage-title">Tus enlaces creados (<?= count($linksData) ?>)</div>
        <?php if (empty($linksData)): ?>
          <p class="hint" style="margin:0;">Aún no has creado enlaces registrados aquí.</p>
        <?php else: ?>
          <?php foreach (array_reverse($linksData, true) as $fid => $item): ?>
            <?php
              $tieneClave = !empty($item['clave_activa']);
              $estaBloqueado = !empty($item['bloqueado']);
              $urlItem = obtenerBaseLimpia($baseUrlSinPanel) . '/' . $publicBaseDir . rawurlencode($fid) . '/';
            ?>
            <div class="link-card">
              <div class="link-head">
                <span><?= htmlspecialchars($fid, ENT_QUOTES, 'UTF-8') ?></span>
                <?php if ($estaBloqueado): ?>
                  <span class="pill pill-ban">Bloqueado</span>
                <?php elseif ($tieneClave): ?>
                  <span class="pill pill-lock">Con clave: <?= htmlspecialchars((string)($item['clave_visible'] ?? '***'), ENT_QUOTES, 'UTF-8') ?></span>
                <?php else: ?>
                  <span class="pill pill-open">Sin clave</span>
                <?php endif; ?>
              </div>
              <div class="link-url"><?= htmlspecialchars($urlItem, ENT_QUOTES, 'UTF-8') ?></div>

              <form method="post" class="ctrl-form">
                <input type="hidden" name="folder_id" value="<?= htmlspecialchars($fid, ENT_QUOTES, 'UTF-8') ?>">
                <input type="text" name="nueva_clave" class="ctrl-input" value="<?= htmlspecialchars((string)($item['clave_visible'] ?? ''), ENT_QUOTES, 'UTF-8') ?>" placeholder="Nueva clave (vacío = quitar)">
                <button type="submit" name="accion_link" value="guardar_clave" class="btn-sm">Poner / Cambiar clave</button>
                <?php if ($tieneClave): ?>
                  <button type="submit" name="accion_link" value="quitar_clave" class="btn-sm btn-gray">Quitar clave</button>
                <?php endif; ?>
                <button type="submit" name="accion_link" value="toggle_bloqueo" class="btn-sm btn-amber"><?= $estaBloqueado ? 'Desbloquear' : 'Bloquear' ?></button>
                <button type="submit" name="accion_link" value="borrar_link" class="btn-sm btn-red" onclick="return confirm('¿Borrar este enlace y sus archivos?');">Borrar</button>
              </form>
            </div>
          <?php endforeach; ?>
        <?php endif; ?>
      </div>
    <?php endif; ?>
  </div>

  <script>
    function copiarLink() {
      const el = document.getElementById('enlaceReal');
      const btn = document.getElementById('copyBtn');
      if (!el) return;
      el.select();
      el.setSelectionRange(0, 99999);
      const copyFn = () => {
        if (btn) {
          const prev = btn.textContent;
          btn.textContent = '✦ Copiado';
          setTimeout(() => { btn.textContent = prev; }, 1400);
        }
      };
      if (navigator.clipboard && navigator.clipboard.writeText) {
        navigator.clipboard.writeText(el.value).then(copyFn).catch(() => {
          document.execCommand('copy');
          copyFn();
        });
      } else {
        document.execCommand('copy');
        copyFn();
      }
    }
  </script>
</body>
</html>
