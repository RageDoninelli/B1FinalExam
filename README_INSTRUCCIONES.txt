V.I.V.A. ACADEMY — B1 FINAL EXAM
Entrega preparada: 27 de octubre de 2026

EXAMEN
- Miércoles 4 de noviembre de 2026.
- 36 preguntas por intento, 3 por cada uno de 12 temas.
- 24 selección múltiple, 6 completar y 6 transformar oraciones.
- 60 minutos por intento, 2 intentos máximos, preguntas distintas en cada uno.
- Nota oficial: la mayor de ambos intentos. Si solo realiza uno, queda su nota del primero.
- La segunda oportunidad se habilita por decisión del docente después de clase.
- El bloqueo y la calificación se ejecutan en Supabase, no en el navegador.

IMPORTANTE: ESTE ZIP NO ES PUBLICABLE TAL CUAL COMO EXAMEN FUNCIONAL.
Requiere crear y configurar Supabase. No se han conectado cuentas ni realizado pruebas
integrales contra una base de datos real. El front-end se validó en sintaxis JavaScript.

INSTALACIÓN (el docente, desde una cuenta propia de Supabase)
1. Crear un proyecto Supabase.
2. SQL Editor: ejecutar supabase/schema.sql y después supabase/questions_seed.sql.
3. Crear estudiantes con códigos individuales; ejemplo:
   insert into public.b1_students(student_id,full_name,pin_hash)
   values ('B1-001','Nombre de prueba',crypt('PIN-LARGO-UNICO',gen_salt('bf')));
   Usar PINs únicos de al menos 10 caracteres; enviarlos individualmente.
4. Crear Edge Function 'exam-api' con el contenido de supabase/functions/exam-api/index.ts.
   Desplegar sin verificación de JWT (por ejemplo: supabase functions deploy exam-api --no-verify-jwt).
   La función verifica el PIN en la base de datos y el panel docente con clave privada.
5. Configurar el secreto de Edge Functions B1_ADMIN_KEY con un valor aleatorio fuerte
   de al menos 24 caracteres. SUPABASE_URL y SUPABASE_SERVICE_ROLE_KEY son secretos
   proporcionados por Supabase al entorno de funciones. NUNCA copiarlos a GitHub.
6. Copiar la URL pública de la función a config.js (único valor editable del frontend).
   Ejemplo: https://PROJECT_REF.supabase.co/functions/v1/exam-api
7. Publicar SOLO los archivos de la raíz: index.html, app.js, config.js,
   style.css, sw.js, manifest.webmanifest, icon.svg, icon-192.png, icon-512.png.
   NO publicar la carpeta supabase/ ni questions.json ni claves de administrador.
8. Antes de la clase, probar con un estudiante ficticio y una base de pruebas;
   verificar cronómetro, reanudación, dos intentos, notas y bloqueo.
9. Abrir primer intento en el SQL Editor el 4 de noviembre:
   update public.b1_settings set exam_open=true,retry_open=false where id=1;
   Después de la clase, habilitar segundo intento:
   update public.b1_settings set retry_open=true where id=1;
   Para cerrar nuevas entradas:
   update public.b1_settings set exam_open=false where id=1;

NOTAS
- El examen no depende de B1Repaso y no modifica sus 219 ejercicios.
- El temporizador corre en el servidor desde el inicio. Si se cierra el navegador,
  el reloj sigue corriendo y el intento puede reanudarse si queda tiempo.
- Cuando vence el tiempo, se envían las respuestas guardadas más recientemente.
- Las preguntas de respuesta escrita se califican por coincidencia normalizada
  (mayúsculas, signos y espacios). Una respuesta alternativa gramaticalmente válida
  podría no reconocerse. El docente debe revisar las respuestas escritas ante disputas.
- El panel docente muestra notas e intentos y exporta CSV. No permite corregir
  manualmente una nota desde la interfaz.
- Para un examen real con muchos alumnos, añadir protección anti-fuerza-bruta,
  límites de solicitudes y revisar políticas de privacidad antes de producción.
- Este proyecto no garantiza evitar colaboración, intercambio de códigos ni trampas.
