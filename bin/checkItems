#!/usr/bin/env node

var pg = require('pg');
var sendgrid  = require('sendgrid')(
  process.env.SENDGRID_USERNAME,
  process.env.SENDGRID_PASSWORD
);

pg.connect(process.env.DATABASE_URL, function(err, client, done) {
  var handleError = function(err) {
    if(!err) return false;
    done(client);
    next(err);
    return true;
  };

  client.query('SELECT * FROM todos', function(err, result) {
    if(handleError(err, client, done)) return;

    if (result.rows.length > 0) {
      sendgrid.send({
          to: 'my@email.com',
          from: 'app@email.com',
          subject: 'There are some items to do',
          text: 'You have items to do'
        }, function(err, json) {
          if (err) { 
            console.error(err); 
          }

          done();
          pg.end();   
      }); 
    }    
  });
});